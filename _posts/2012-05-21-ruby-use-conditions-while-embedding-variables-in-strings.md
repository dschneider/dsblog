---
layout: post
title: Ruby - Use conditions while embedding variables in strings
tags:
- ruby
- strings
- conditions
- embedded
- variables
---
I found out about another neat little detail in Ruby. Have you ever been
working with strings and ending up using conditions to generate a different
output based on some user attribute for example?

{% gist 2941773 %}

or

{% gist 2941809 %}

or something similar? Well, there is a simpler way to achieve the same
behavior:

{% gist 2941811 %}

You can directly place the condition within the string when using #{}. I just
love this beautiful one-liner and it’s still very understandable.

