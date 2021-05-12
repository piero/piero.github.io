---
layout: post
title:  "There's something about Clojure (part 3)"
date:   2021-05-05 22:34:00 +0100
category: clojure
tags: clojure programming languages
---

## Expressiveness

I remember the first person who showed Clojure to me saying enthusiastically how it was "such an expressive language".
Back then, I didn't really understand what he meant.

I currently work a lot with time series coming from sensor data, in example a stream of temperatures like this one:

```json
[{"ts": "2021-04-23T12:16:00Z",
  "avg-temperature": 20.1},
 {"ts": "2021-04-23T12:51:00Z",
  "avg-temperature": 20.0},
 {"ts": "2021-04-23T13:11:00Z",
  "avg-temperature": 20.4}]
```

That can be represented in the equivalent [EDN](https://github.com/edn-format/edn) format:

```clojure
[{:ts "2021-04-23T12:16:00Z"
  :avg-temperature 20.1}
 {:ts "2021-04-23T12:51:00Z"
  :avg-temperature 20.0}
 {:ts "2021-04-23T13:11:00Z"
  :avg-temperature 20.4}]
```

Given a stream like that, I want to compute some statistics out if it.
