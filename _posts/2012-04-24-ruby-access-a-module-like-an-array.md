---
layout: post
title: Ruby - Access a module like an array
tags:
---
I found a cool way to access a module like an array:

{% gist 2941908 %}

**self::[]** enables you to treat the whole module like an array. You could then say Fruits[0] and it would show the element at the index 0, which is "Banana". Very handy!

Hint: Instead of using **self::[]** you could also use **self.[]**. However
the former is cleaner to read in my opinion.

