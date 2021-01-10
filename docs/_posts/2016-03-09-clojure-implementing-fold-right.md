---
layout: post
title: "Clojure: implementing foldr"
date: 2016-03-09
author: Kean Santos
---

## Foldr: recursive implementation

Fold right takes 3 arguments: a function, a sequence, and an initial value; then it keeps nesting calls to the function until the last element is reached. 

    (defn foldr-rec
    " Naive recursion 
      f -- the function to apply
      [x & xs] -- the sequence, which i've broken up into the first and rest
      z  -- the initial / 'zero' value"
      [ f  [ x & xs ] z ] 
      ( if-not ( nil? x )
         ( f x ( foldr-rec f xs z ) ) 
         z))
 
I'm following the Scala (or Haskell, or both; don't quite remember) notation of passing the initial value as the right-most argument of fold-right, to remind myself (and users) the the function "f" takes the initial value "z" as the argument on the right. If the input is not empty, call f(1, f(2, ...)) . When the input is exhausted, the z value is returned, meaning the last element is processed using f(4, z).

    (foldr-rec vector [1 2 3 4] [])
    => [1 [2 [3 [4 []]]]]

The shortcoming is that it gets a stack overflow:

    (def r (foldr-rec vector (range 10000) []))
    => java.lang.StackOverflowError

Next up: implementation with 'reverse'.

