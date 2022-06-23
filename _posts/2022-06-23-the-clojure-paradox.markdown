---
layout: post
title:  "The Clojure paradox"
date:   2022-06-23 08:00:00 +0100
category: stories
tags: clojure programming
---

The new [StackOverflow Developer Survey](https://survey.stackoverflow.co/2022/) is out.

Once again, a large proportion of respondents is made of people with less than
15 years of coding experience and less than 10 years of professional work experience:

![Years coding](/assets/posts/2022-06-23/years-coding.png)

![Years of professional experience](/assets/posts/2022-06-23/years-of-professional-experience.png)

As such, that survey is very biased and doesn't represent a reliable sample of software developers.

### Nevertheless...

Once again, Clojure is in the long tail of [language popularity]
(https://survey.stackoverflow.co/2022/#technology-most-popular-technologies),
scoring only 1.51% overall and 1.66% among professional developers.

Once again, though, despite its low popularity, Clojure is one of the __most loved languages__:

![Most loved languages](/assets/posts/2022-06-23/most-loved-languages.png)

And once again, Clojure is also the __top paying language__:

![Top paying languages](/assets/posts/2022-06-23/top-paying-languages.png)

### Isn't this a paradox?

Time for some light-hearted data science: let's see the ratio of `love / popularity` percentages.
Among professional developers, some of the languages score like this:

| Language   | Love % | Popularity % | Ratio  |
|:-----------|-------:|-------------:|-------:|
| JavaScript |  61.46 |        67.90 |  0.905 |
| Python     |  67.34 |        43.51 |  1.547 |
| TypeScript |  73.46 |        40.08 |  1.832 |
| Go         |  64.58 |        11.83 |  5.459 |
| Rust       |  86.73 |         8.80 |  9.856 |
| ...        |    ... |          ... |    ... |
| Elixir     |  75.46 |         2.46 | 30.675 |
| Clojure    |  75.23 |         1.66 | 45.319 |

It looks like the love for Elixir and Clojure isn't due to popularity, it appears to be _true love_!!

One way I explain Clojure's low popularity score in the StackOverflow survey is that most respondents
are junior-mid developers, who may not get past the LISP syntax and the functional style.
Clojure was written by an experienced developer ([Rich
Hickey](https://en.wikipedia.org/wiki/Rich_Hickey)) and is adopted mostly by experienced developers.
It's not beginner-friendly, it's not perfect either. However, it is _extremely pragmatical_.

Also, Clojure is used by high-stakes companies such as [JUXT](https://www.juxt.pro/) and
[Nubank](https://nubank.com.br/en/), there _must_ be something really good about it.

My gut feeling is that __the appreciation of pragmatism over perfectionism is an acquired taste__,
which takes time and experience to develop. Clojure's low popularity in the StackOveflow survey may be
due to the low number of experienced developers who took part in that poll, and it is remarkable how
the love for Clojure came out despite its user base being so under-represented.

### In my little experience...

I wish that I could write Clojure when I am not writing it. After a while it becomes such a natural
way of manipulating data structures and expressing complex concepts easily, without having to create
unnecessary abstractions (I'm looking at you Java).

Also, a programming life without classes is a happier life: I would have never thought I'd say such a
thing 5 years ago, life is so impredictable.

