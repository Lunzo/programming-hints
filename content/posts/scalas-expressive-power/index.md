---
title: "Scala's Expressive Power"
date: 2016-02-20T05:57:00
tags: [puzzles, scala]
---
I saw the following puzzle the other day. Given a set of rings, can you place the numbers 1 through 9 inside each closed area so that the sum of the numbers in each circle is the same?

![Olympic Rings Puzzle](olympicring.png "Olympic Rings Puzzle")

The 1 was already placed for you.

Because I'm trying to learn Scala, I thought writing a program to solve this would be good practice for me. I assume that the numbers in each closed area are stored in an `Array` with elements in the array corresponding to the closed areas from left to right.

```
def isValid(values: IndexedSeq[Int]) = {
    val target = values.take(2).sum
    values.slice(1, 4).sum == target &&
    values.slice(3, 6).sum == target &&
    values.slice(5, 8).sum == target &&
    values.slice(7, 9).sum == target    
}
```

This was dead easy to write. It's just encoding the solved condition from the question. Next was the tricky part -- going through all permutations of 2 to 9 with 1 in a fixed place until we arrived at an answer. I remember writing code to generate all permutations once as an interview question and it isn't easy. You have to write a complicated recursive function to generate the permutations.

Thankfully a few minutes spent in the ScalaDoc allowed me to use some of the powerful functions provided by Scala out of the box. Here's code to solve the problem in a handful of lines.

```
val available = 2 to 9

for (perm <- available.permutations) {
    val values = (perm.take(3) :+ 1) ++: perm.drop(3)
    if (isValid(values)) {
        println("Winner:")
        println(values.mkString(", "))        
    }
}
```

Yes, there's a function to give you all permutations of a sequence. The `:+` and `++:` are operators that join an element to a sequence and join two sequences respectively.

If you want the answer you'll have to download Scala and run the above program. Or you can try solving it yourself in the language of your choice.