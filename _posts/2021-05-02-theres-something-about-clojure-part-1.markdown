---
layout: post
title:  "There's something about Clojure (part 1)"
date:   2021-05-02 22:34:00 +0100
category: clojure
tags: clojure programming languages
---

*This is not an introduction to Clojure, there's [plenty](http://clojure-doc.org/articles/tutorials/introduction.html) of [excellent](https://www.braveclojure.com/) [ones](https://www.clojure.org/guides/getting_started) already. These are my reflections on why I felt in love with Clojure, although it took some time to build that relation, as a reminder for my future self, should it become frustrated by it at some point.*


## Dear future self...

...it was only about six years ago that I came across Clojure. At that time I had over 15 years of C++ and embedded Linux systems under my belt, together with a common mix of other languages such as Java, Python, and Go. Although I've had a very brief encounter with Scala, I considered functional programming some exot(er)ic field, certainly not something for professional use. How wrong I was! Yet I didn't know it when I first saw Clojure, just a snippet of code in the company I was working for at that time. I remember the sensation of being almost blind, of not being able to make sense of that weird syntax. I certainly couldn't see myself writing that stuff for a living, and yet life can be so ironic.


## Parentheses

Clojure is a dialect of Lisp, which is famous for its [parentheses](https://xkcd.com/297/). I didn't know that When I first saw it, so I just thought it was one of those write-once/read-never academic languages. And again, how wrong I was! Let me explain things here as a reminder to you, my future self, because [I have now seen the light](https://xkcd.com/224/).

Here's a "traditional" function invocation:

```c
print("Hello world")
```

Now let's move the parentheses a bit;

```clojure
(print "Hello world")
```

It's the same thing but Ta-Daa, welcome to the Lisp syntax! Well, it's [a bit more complicated](https://en.wikipedia.org/wiki/S-expression) than that, but you get the point.

No blocks delimited by threatening sharp curly braces:

```go
func crunchNumbers(x int, y int) int {
    if x <= y {
        return x + y
    }
    return x - y
}
```

Instead sequences of sinuous, fluffy, round parentheses:

```clojure
(defn crunch-numbers [x y]
    (if (<= x y) (+ x y) (- x y)))
```

So relaxing!


## Immutability

Functional languages are famous because they don't have side effects, or at least they do their best to discourage them, nudging programmers toward writing "pure" functions: no side effects -> idempotent execution -> parallelise at will -> big win!

The Go function above deliberately creates a new array, without modifying the input one: this is to mimic the same behaviour as Clojure. Otherwise, a simpler version would be:

```go
func SquareArrayMut(a []int) {
    for i := 0; i < len(a); i++ {
        a[i] = a[i] * a[i]
    }
}

a := []int{0, 1, 2, 3, 4}
SquareArrayMut(a)
fmt.Println(a)  // prints [0 1 4 9 16]
```

After calling `SquareArrayMut` the array `a` has changed, its original values are no more. In a more complex scenario where a resource is shared among threads, that could lead to hard to spot bugs if not properly protected, e.g. with a mutex. But if data structure is immutable, it can be safely shared and each thread can use it at will.

```clojure
(def a (range 5))  ;; a = (0 1 2 3 4)

(def squared-a (square a))  ;; squared-a = (0 1 4 9 16)

(println a)  ;; prints (0 1 2 3 4)
```

Nothing unexpected. Honest. Beautiful.


## Coinciseness

Let's write a function that squares the numbers of an array.

In some imperative language, e.g. Go:

```go
func SquareArray(a []int) []int {
    result := make([]int, len(a))

    for i := 0; i < len(a); i++ {
        result[i] = a[i] * a[i]
    }

    return result
}

a := []int{0, 1, 2, 3, 4}
fmt.Println(SquareArray(a))  // [0 1 4 9 16]
```

In Clojure:

```clojure
(defn square [input]
    (map (fn [i] (* i i)) input))

(square (range 5))  ;; prints (0 1 4 9 16)
```

One line. No assignments. Simple. Elegant. Beautiful.

Here [`map`](https://clojuredocs.org/clojure.core/map) simply applies the function `(fn [i] (* i i))` to the items of `input`, which is assumed to be a [collection](http://clojure-doc.org/articles/language/collections_and_sequences.html), and returns a collection with the result. The multiplication is writted in Polish notation simply because `*` is not an operator, but a function (so consistent!). The arguments of a function are wrapped in square brackets because they form a `vector`: this also happens in other languages (e.g. `argv` in C/C++, or `$@` in bash) but in Lisp/Clojure it's explicit. Because... [Code is Data, Data is Code](https://en.wikipedia.org/wiki/Homoiconicity). Oh beauty, oh perfection!


## Next time...

I remember the first person who showed Clojure to me saying enthusiastically how it was "such an expressive language".
back then, I didn't really understand what he meant by that.
