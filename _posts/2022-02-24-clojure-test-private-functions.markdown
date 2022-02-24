---
layout: post
title:  "Testing private functions in Clojure"
date:   2022-02-24 13:26:00 +0100
category: clojure
tags: clojure programming tips testing
---

Let's suppose there's this function:

```clojure
;; src/mypackage.clj
(ns mypackage)

(defn- my-private-function
  [x]
  ;; For simplicity, just return the input
  x)
```

You can test it like this:

```clojure
;; test/mypackage_test.clj
(ns mypackage-test
  (:require [mypackage :as p]))

(deftest test-my-package
  (let [function-to-test (var p/my-private-function)]
  
    (testing "happy path"
      (is (= 42 (function-to-test 42))))))
```

