---
layout: post
title: "Trying out Clojure"
date: 2015-10-30
---

# Trying out Clojure

I'm reviewing some basic software algorithms, and writing them out with both Scala and Clojure.

My first shot is at the merge sort algorithm, which can be summarized as:
1. Repeatedly break the input array into smaller pieces
2. Merge them together by taking the lowest element of each array and putting it into the output

My study materials use C and Java to implement these algorithms, and I initially found myself thinking in terms of 'for' loops when implementing the merge step -- traverse 2 arrays, comparing each element and writing the smaller one to the output array.

This implementation involves mutable objects: counter variables, as well as a mutable output vector, which are discouraged in Clojure. To write the code using immutable values, I had to do away with this thinking  and I started with a recursive implementation.

I was expecting it to be a mess, t came out better than I thought -- the code reflects the algorithm  with reasonable closeness:

* Merge lists by taking the first element of each list and comparing them
* Keep taking elements until the list is empty
The actual merge sort method is the last function in the listing:
* Split the list into two.
* Merge the sorted halves.

On the down side, this probably can't handle a large number of inputs without a stack overflow.

Here is the my simple merge sort code:

    (ns test-mergesort)

    (declare merge)
    ; merge 2 non-empty lists in ascending order
    (defn- merge-nonempty [ [l & ll :as left ] [r & rr :as right] ]
         (if (<= l r)
           (into [l] (merge ll right))
           (into [r] (merge left rr)) ))

    ; merge 2 lists either of which may be empty
    (defn- merge [left right]
      (println "Merging input: " left " , " right)
      (cond (empty? left) right
            (empty? right) left
            :else (merge-nonempty left right) ))

    ; mergesort implementation: keep splitting input into 2, then merge outputs.
    (defn mergesort "experimental merge sort implementation" [v]
      (println "Sorting input: " v)
      (if (<= (count v) 1) v
        (let [len (count v)
              mid (/ len 2)
              left (subvec v 0 mid)
              right (subvec v mid)]
          (merge (mergesort left) (mergesort right)))))
    (println "Result: " (mergesort [3 5 1 2 9 5 7 4]))

The last line of the listing calls mergesort on a vector of 8 elements.
I'd expect n log2 n complexity for this, which roughly corresponds to the number of print lines in the output:

    Sorting input:  [3 5 1 2 9 5 7 4]
    Sorting input:  [3 5 1 2]
    Sorting input:  [3 5]
    Sorting input:  [3]
    Sorting input:  [5]
    Merging input:  [3]  ,  [5]
    Merging input:  nil  ,  [5]
    Sorting input:  [1 2]
    Sorting input:  [1]
    Sorting input:  [2]
    Merging input:  [1]  ,  [2]
    Merging input:  nil  ,  [2]
    Merging input:  [3 5]  ,  [1 2]
    Merging input:  [3 5]  ,  (2)
    Merging input:  [3 5]  ,  nil
    Sorting input:  [9 5 7 4]
    Sorting input:  [9 5]
    Sorting input:  [9]
    Sorting input:  [5]
    Merging input:  [9]  ,  [5]
    Merging input:  [9]  ,  nil
    Sorting input:  [7 4]
    Sorting input:  [7]
    Sorting input:  [4]
    Merging input:  [7]  ,  [4]
    Merging input:  [7]  ,  nil
    Merging input:  [5 9]  ,  [4 7]
    Merging input:  [5 9]  ,  (7)
    Merging input:  (9)  ,  (7)
    Merging input:  (9)  ,  nil
    Merging input:  [1 2 3 5]  ,  [4 5 7 9]
    Merging input:  (2 3 5)  ,  [4 5 7 9]
    Merging input:  (3 5)  ,  [4 5 7 9]
    Merging input:  (5)  ,  [4 5 7 9]
    Merging input:  (5)  ,  (5 7 9)
    Merging input:  nil  ,  (5 7 9)
    Result:  [1 2 3 4 5 5 7 9]

I've got about 38 print lines, which is closer to 24 = 8 * log2 8 than either 8 (linear) or 64 (quadratic). 

I'm planning to do 2 things next:

1. test with a large number of inputs and fix up stack overflow exceptions, and
2. post a scala implementation.
