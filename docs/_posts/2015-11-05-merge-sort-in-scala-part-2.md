---
layout: post
title: "Merge sort in Scala, part 2"
date: 2015-11-05
author: Kean Santos
---

Here is the complete mergesort implementation in scala.

This function is simpler than its clojure counterpart -- val declarations and return statements make things shorter.

    import scala.annotation.tailrec

    object Worksheet2 {
      println("Welcome to the Scala worksheet")       //> Welcome to the Scala worksheet
      
      def mergesort(v: Vector[Int]): Vector[Int] = {
        val len = v.length
        if (len <= 1) return v
        val mid = len / 2
        val left = v take mid
        val right = v drop mid
        merge(Vector(), mergesort(left), mergesort(right));
      }                                               //> mergesort: (v: Vector[Int])Vector[Int]

      //@tailrec
      def merge(acc: Vector[Int], left: Vector[Int], right: Vector[Int]): Vector[Int] = {
        //println("Merging input: " + left + " , " + right)
        val (nextMin, nextLeft, nextRight) = (left, right) match {
          case (l +: ll, r +: rr)  => if (l < r) (l, ll, right) else (r, left, rr);
          case (Vector(), r +: rr) => return acc ++ right;
          case (l +: ll, Vector()) => return acc ++ left;
        }
        merge(acc :+ nextMin, nextLeft, nextRight)
      }                                               //> merge: (acc: Vector[Int], left: Vector[Int], right: Vector[Int])Vector[Int]

      val v = Vector(3,5,1,2,9,5,7,4)                 //> v  : scala.collection.immutable.Vector[Int] = Vector(3, 5, 1, 2, 9, 5, 7, 4)
                                                      //| 
                          v take v.length /2          //> res0: scala.collection.immutable.Vector[Int] = Vector(3, 5, 1, 2)
                          v drop v.length /2          //> res1: scala.collection.immutable.Vector[Int] = Vector(9, 5, 7, 4)
                          
      val m = mergesort(v)                            //> m  : Vector[Int] = Vector(1, 2, 3, 4, 5, 5, 7, 9)
    }

