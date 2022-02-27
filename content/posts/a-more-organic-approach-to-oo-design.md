---
title: "A more organic approach to Object-Oriented design"
date: 2014-11-23T10:43:00
tags: [article, "object oriented design", design]
---
Today's article I'd like to share with you is: [Semantic Compression](https://caseymuratori.com/blog_0015).

The title doesn't say much about the content of the article to someone who hasn't read it, so I'll give a brief overview.

The author demonstrates his approach to object-oriented design through a very detailed programming example. Instead of starting with a design based on real world entities, he argues that you should start by adding the features you need in procedural style code. Every time you start repeating yourself, you'll be able to see what the repetition has in common. It's only when you have two or more concrete examples of repetition hat you can then make design choices about re-use through classes and functions.

By programming in this manner you avoid designing too much flexibility too soon. You create just the abstractions you need instead of over-engineering your code based on a design that doesn't match the goals of your program. Having worked on a recent project in a similar manner, I can say that I'm in agreement with the author. This approach to programming seems to be quite effective.