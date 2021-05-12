---
layout: post
title:  "Multiple definitions in Clojure"
date:   2021-05-12 12:34:00 +0100
category: clojure
tags: clojure programming tips
---

I often have to execute functions step-by-step in the REPL, which can be a problem
if there are `let` statement with many bindings.

In example:

```clojure
(defn analyse-movement-insights
  [insights]
  (let [average-count (analysis-tools/average (map #(get-in % [:insights :total-count]) insights))
        average-expected-min (analysis-tools/average (map #(get-in % [:insights :expected-min]) insights))
        num-unexpected-days (count (filter #(= % 1) (map #(get-in % [:insights :day-rating]) insights)))
        num-at-risk-days (count (filter #(= % 2) (map #(get-in % [:insights :day-rating]) insights)))
        average-first-event-ts (average-timestamp-hours (map #(get-in % [:insights :first-event-ts]) insights))
        average-last-event-ts (average-timestamp-hours (map #(get-in % [:insights :last-event-ts]) insights))
        ...
        ]

    ...

    ))
```

It's really tedious to run each line in the REPL, like:

```clojure
(def average-count (analysis-tools/average (map #(get-in % [:insights :total-count]) insights)))
=> #'metrics.daily-hub-insights/average-count

(def average-expected-min (analysis-tools/average (map #(get-in % [:insights :expected-min]) insights)))
=> #'metrics.daily-hub-insights/average-expected-min

...
```

So I just define this macro in the REPL:

```clojure
(defmacro defs
  [& bindings]
  {:pre [(even? (count bindings))]}
  `(do
     ~@(for [[sym init] (partition 2 bindings)]
         `(def ~sym ~init))))
```

and then it's just a single copy/paste:

```clojure
(defs average-count (analysis-tools/average (map #(get-in % [:insights :total-count]) insights))
      average-expected-min (analysis-tools/average (map #(get-in % [:insights :expected-min]) insights))
      num-unexpected-days (count (filter #(= % 1) (map #(get-in % [:insights :day-rating]) insights)))
      num-at-risk-days (count (filter #(= % 2) (map #(get-in % [:insights :day-rating]) insights)))
      average-first-event-ts (average-timestamp-hours (map #(get-in % [:insights :first-event-ts]) insights))
      average-last-event-ts (average-timestamp-hours (map #(get-in % [:insights :last-event-ts]) insights)))
```

Credits: [https://stackoverflow.com/questions/35539956/defining-multiple-constant-variables-in-clojure](https://stackoverflow.com/questions/35539956/defining-multiple-constant-variables-in-clojure)
