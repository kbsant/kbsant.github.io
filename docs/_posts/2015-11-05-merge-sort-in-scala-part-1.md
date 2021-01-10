---
layout: post
title: "Merge sort in Scala, part 1"
date: 2015-11-05
author: Kean Santos
---

Here is the implementation of the merge routine in scala. I basically translated it from clojure code. It is reasonably readable, but I had a tough time writing it (kept having to look up the syntax). However, I'm unsure if short-circuiting the pattern match with return statements is good form. 

The listing below includes the output from the friendly scala worksheet:

    import scala.annotation.tailrec

    object Worksheet2 {
      println("Welcome to the Scala worksheet")       //> Welcome to the Scala worksheet
      //@tailrec
      def merge(acc: Vector[Int], left: Vector[Int], right: Vector[Int]): Vector[Int] = {
        println("Merging input: " + left + " , " + right)
        val (nextMin, nextLeft, nextRight) = (left, right) match {
          case (l +: ll, r +: rr)  => if (l < r) (l, ll, right) else (r, left, rr);
          case (Vector(), r +: rr) => return acc ++ right;
          case (l +: ll, Vector()) => return acc ++ left;
        }
        merge(acc :+ nextMin, nextLeft, nextRight)
      }                                               //> merge: (acc: Vector[Int], left: Vector[Int], right: Vector[Int])Vector[Int]

      val l = Vector(1, 5, 9)                         //> l  : scala.collection.immutable.Vector[Int] = Vector(1, 5, 9)
      val r = Vector(3, 4, 10)                        //> r  : scala.collection.immutable.Vector[Int] = Vector(3, 4, 10)
      val m = merge(Vector(), l, r)                   //> Merging input: Vector(1, 5, 9) , Vector(3, 4, 10)
                                                      //| Merging input: Vector(5, 9) , Vector(3, 4, 10)
                                                      //| Merging input: Vector(5, 9) , Vector(4, 10)
                                                      //| Merging input: Vector(5, 9) , Vector(10)
                                                      //| Merging input: Vector(9) , Vector(10)
                                                      //| Merging input: Vector() , Vector(10)
                                                      //| m  : Vector[Int] = Vector(1, 3, 4, 5, 9, 10)
    }


