---
layout: post
title: "Immutability and State"
date: 2015-11-25
author: Kean Santos
---

Until recently, most of my exploration of Clojure has been with 'large functions' (as described on <https://clojure.org/state>). My largest program was a sudoku solver, and all it returned was a list of values representing solutions to the sudoku board.

I finally had to deal with state when writing my role-playing game at <httpss://www.ersbane.com> (still at a very early stage). In imperative programming, I would handle changes to creatures, rooms and items by directly updating values, and possibly wrapping those updates with locks. In clojure, I apply functions to do the updates. These functions are composable and are applied using a CAS operation, that ensure consistency over time. A much better explanation is at the Clojure for the Brave and True website: <https://www.braveclojure.com/zombie-metaphysics/>.


