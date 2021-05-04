---
layout: post
title:  "There's something about Clojure (part 2)"
date:   2021-05-05 22:34:00 +0100
category: clojure
tags: clojure programming languages
---

## Expressiveness

I remember the first person who showed Clojure to me saying enthusiastically how it was "such an expressive language". Back then, I didn't really understand what he meant.

I recently read [this post](https://ggcarvalho.dev/posts/montecarlo/) about running Monte Carlo simulations
in Go and thought "how would it translate to Clojure?". Let's see.

This is the original code from that post:

```go
package main

import (
    "fmt"
    "math"
    "math/rand"
    "time"
)

func main() {
    rand.Seed(time.Now().UTC().UnixNano())

    const points = 1_000_000
    fmt.Printf("Estimating pi with %d point(s).\n\n", points)

    var sucess int
    for i := 0; i < points; i++ {
        x, y := genRandomPoint()

        // Check if point lies within the circular region:
        if x*x+y*y < 1 {
            sucess++
        }
    }

    piApprox := 4.0 * (float64(sucess) / float64(points))
    errorPct := 100.0 * math.Abs(piApprox-math.Pi) / math.Pi

    fmt.Printf("Estimated pi: %9f \n", piApprox)
    fmt.Printf("pi: %9f \n", math.Pi)
    fmt.Printf("Error: %9f%%\n", errorPct)
}

// generates a random point p = (x, y)
func genRandomPoint() (x, y float64) {
    x = 2.0*rand.Float64() - 1.0
    y = 2.0*rand.Float64() - 1.0
    return x, y
}
```

Here is my Clojure implementation:

```clojure
(ns montecarlo-pi.core
  (:gen-class))

(defn- generate-random-point
  []
  [(dec (* 2.0 (rand))) (dec (* 2.0 (rand)))])

(defn- run-simulation
  [points]
  (loop [points-left points
         successes 0]
      (if (pos? points-left)
        (let [[x y] (generate-random-point)]

          (recur (dec points-left)
                 (if (< (+ (* x x) (* y y)) 1)
                   (inc successes)
                   successes)))
        successes)))

(defn -main
  [& args]
  (let [points 1000000
        _ (println (format "Estimating pi with %d point(s)." points))
        successes (run-simulation points)
        estimated-pi (* 4.0 (/ successes points))]

    (println "Estimated pi:" estimated-pi)
    (println "pi:" Math/PI)
    (println "Error:" (/ (* 100.0 (Math/abs (- estimated-pi Math/PI))) Math/PI))))
```

A few points about the Clojure code:

1) No imports. Clojure is not a system language, many things are available as part of `clojure.core`.
2) No `for` loop. I used `loop/recur` instead, which prevents having some `i` variable that is not used for computation.
3) No assignments. The `let` statements only creates lexical bindings within the scope of that block. No state is ever mutated.
4) Coincise syntax. See the `(let [[x y] (generate-random-point)] ...)` statement? That's [destructuring](https://clojure.org/guides/destructuring), one of my favourite Clojure features.
5) Direct calls to Java. Clojure runs on JVM and can use whatever is available in Java, e.g. `Math/abs` and `Math/PI`.

