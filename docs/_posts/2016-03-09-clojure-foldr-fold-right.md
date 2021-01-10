---
layout: post
title: "Clojure: foldr (fold right)"
date: 2016-03-09
author: Kean Santos
---

While reading the [Joy of Clojure](http://www.joyofclojure.com/), I came across a sample problem to help illustrate lazy evaluation. The problem was: given a sequence,

    [ 1 2 3 4 ]

write a function to transform it into the nested structure below.

    [ 1 [ 2 [ 3 [ 4 [] ] ] ] ]

Seeing that fold-right would fit the bill, I started to search for it in the Clojure docs, but it turns out that Clojure only has either fold-left functions,or reduction functions where elements in a collection don't matter.
For example,

    ( reduce + [ 1 2 3 4 ] )

would return the same result regardless of the order of the elements, since addition is commutative.

## Fold-left vs Fold-right

However, doing the following

    ( reduce vector  []  [ 1 2 3 4 ] )  

would result in

    [ [ [ [ [] 1 ] 2 ] 3 ] 4 ]

since reduce works from the left of the collection (same as fold-left).
Left-fold applies the function f with initial value z to a collection [ 1 2 3 4 ] as follows:

    f(z, 1)  ;; this is evaluated, and then re-used in the next iteration
    f(f(z,1), 2)
    f(f(f(z,1), 2), 3)
    f(f(f(f(z,1), 2), 3), 4)

It's easily implemented using a "for" iteration, since each value can be calculated and accumulated before moving to the next iteration.
Fold-right requires the last element in the collection to be evaluated, before it can evaluate any preceding elements:

    f(1,*)  ;; to evaluate this, need to plug in the result of "f" on the next element
    f(1,f(2,*))
    f(1,f(2,f(3,*)))
    f(1,f(2,f(3,f(4,*))))
    f(1,f(2,f(3,f(4,z))))

Note that this closely resembles the desired result from the example:

    [ 1 [ 2 [ 3 [ 4 [] ] ] ] ]

In my next post, I'll share my 3 attempts at implementing fold-right.

