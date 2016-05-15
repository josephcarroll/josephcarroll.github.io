---
layout: post
title:  "A Litmus Test"
date:   2016-05-14 20:00:00 -0400
categories: jekyll update
tags:
 - Scala
 - Java
 - Interview
---

If you're interviewing for a junior-ish position as a software developer, I'm hoping this article will provide you insight into some of the common pitfalls I see interviewees falling into. In other words - key things to avoid when asked to solve an algorithmic problem!

The Problem
===========

When I interview a candidate for a internship or graduate position, I always start with the same question:

> Ok, so here's the problem. Given a list of words, let's say the words from War and Peace - I want you to give the me the unique words. That is to say, I want the words that appear exactly once.
> 
> So for example, the word 'the' will appear hundreds of times, so I'm not interested in that. But a word like 'derivative' might only appear once.

I ask the candidate to explain their thought process, talk me through a rough algorithm for producing the result, and write some pseudo code to solve the problem. An example input and solution (coming from a Scala developer like myself) would be:

{% highlight scala %}
val input = Seq("cow", "cat", "horse", "cow", "horse", "dog")
val unique = input.groupBy(identity).collect { 
  case (word, count) if count.length == 1 => word 
}
println(unique) /* List(dog, cat) */
{% endhighlight %}

_* In case you haven't seen it before, `identity` is a neat shortcut for `x => x` in our group by._

The Usual Solution
==================

The good news is that 80% of the candidates I interview (who tend to have a Java or Python background) are able to produce an answer using a HashMap reasonably quickly:

{% highlight java %}
public List<String> findUniqueWords(List<String> input) {
    Map<String, Int> words = new HashMap<String, Int>();
    for(String word : input) {
        if (words.containsKey(word)) {
            words.put(word, words.get(word) + 1);
        } else {
            words.put(word, 1);
        }
    }
    List<String> results = new ArrayList<String>();
    for(Entry<String, Int> entry : words) {
        if (entry.getValue() == 1) {
            results.add(entry.getKey());
        }
    }
    return results;
}
{% endhighlight %}

It's not quite as terse as the Scala solution above, but it gets the job done, is efficient - and most importantly of all, allows me to segway into asking about time complexity and why HashMaps have constant time operations for get and put.

Common Pitfalls
===============

If you've gotten this far, we're now getting to the good stuff. Here are some of the stumbling blocks I see the other 20% of candidates hitting when these simple problems with pseudo code:

 * *Not understanding the question*

   Too often, a candidate will hear the word unique and stop listening. They'll then tell me a set will solve the problem by making the input list unique. If you're unclear about the question (or it seems a little bit too easy) ask the interviewer to clarify. Better yet, ask for an example input/output!

 * *Getting bogged down in syntax*

   Given that I'm asking for code written on paper - I'm not expecting it to compile! The solution doesn't have to be syntactically correct Java, and no points will be lost for missing semicolons.

   I also see candidates writing out long form version of generic signatures and other boilerplate. Given the length of time available for an interview, it's ok to skip this and just verbally confirm with the interviewer that a variable is of a certain type.

 * *Not using common data structures*

   The nice solution for this problem I usually see involes a HashMap (or Dictionary). If you try and solve this problem (and other similar questions) without one - you're going to have a bad time. The naive solution in this case involves an **O(n<sup>2</sup>)** comparison of all words with all other words. Candidates who start writing this solution tend to rush in and start writing out by one errors as they realize you have to avoid comparing each word with itself.

Summary
=======

This article is called 'A Litmus Test' because I use my unqiue words problem to filter out candidates who can't write basic pseudo code or who struggle with basic comprehension. If you listen carefully to instructions (asking for clarification when needed), don't worry too much about compilation and semicolons, and make sure you make the most of common data structures like maps and sets - you can focus more on problem solving, and advancing to the more interesting parts of a technical interview.