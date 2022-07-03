---
layout: post
title: Implicits in Scala - basics
subtitle: Two basic types
cover-img: /assets/img/implicits.jpg
thumbnail-img: /assets/img/implicits.jpg
share-img: /assets/img/implicits.jpg
gh-repo: bartekmaciejewski/bartekmaciejewski.github.io
gh-badge: [star, fork, follow]
tags: [implicits, scala, basics]
comments: true
---

## Implicits in Scala explained

https://www.youtube.com/watch?v=6tiPdV93Shw


### Unexpected behaviour (aka why be cautious using implicits)

```
implicit val l: Seq[String] = List("a", "12", "78")
val a: String = 2
```

Implicit conversion at its best... :)