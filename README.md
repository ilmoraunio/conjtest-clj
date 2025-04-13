# conjtest-clj

- [Project status](#project-status)
- [Usage](#usage)

Validate data structures against policies.

Conjtest-clj is a Clojure/babashka library for validating parsed data
structures against policies, which are just Clojure functions.

Part of the [conjtest](https://github.com/ilmoraunio/conjtest) project.

## Project status

[![Clojars Project](https://img.shields.io/clojars/v/org.conjtest/conjtest-clj.svg)](https://clojars.org/org.conjtest/conjtest-clj)
[![Slack](https://img.shields.io/badge/slack-conjtest-orange.svg?logo=slack)](https://clojurians.slack.com/app_redirect?channel=conjtest)
[![cljdoc badge](https://cljdoc.org/badge/org.conjtest/conjtest-clj)](https://cljdoc.org/d/org.conjtest/conjtest-clj)
[![bb compatible](https://raw.githubusercontent.com/babashka/babashka/master/logo/badge.svg)](https://book.babashka.org#badges)

Project is **active** and in
[alpha](https://kotlinlang.org/docs/components-stability.html#stability-levels-explained).

Check [CHANGELOG.md](CHANGELOG.md) for any breaking changes.

## Usage

[API documentation](https://cljdoc.org/d/org.conjtest/conjtest-clj)

```bash
clj -Sdeps '{:deps {org.conjtest/conjtest-clj {:mvn/version "0.3.1"} org.flatland/ordered {:mvn/version "1.15.12"}}}'
```

```clojure
(require '[conjtest.core :as conjtest]
         '[flatland.ordered.map :refer [ordered-map]])

(set! *data-readers* {'ordered/map #'flatland.ordered.map/ordered-map})

(def parsed-yaml
  {"test-resources/test.yaml" #ordered/map([:apiVersion "v1"]
                                           [:kind "Service"]
                                           [:metadata #ordered/map([:name "hello-kubernetes"])]
                                           [:spec
                                            #ordered/map([:type "LoadBalancer"]
                                                         [:ports (#ordered/map([:port 9999] [:targetPort 8080]))]
                                                         [:selector #ordered/map([:app "hello-kubernetes"])])])})

(defn deny-allow-only-port-80
  [input]
  (and (= "v1" (:apiVersion input))
       (= "Service" (:kind input))
       (not= 80 (-> input :spec :ports first :port))))

(conjtest/test! parsed-yaml deny-allow-only-port-80)
;; Execution error (ExceptionInfo) at conjtest.core/test-with-opts! (core.clj:284).
;; FAIL - test-resources/test.yaml - :conjtest/rule-validation-failed
;;
;; 1 tests, 0 passed, 0 warnings, 1 failures
```

Instead of a map, where the keys should denote the parsed data structure's
location on the file system, you may instead pass `conjtest/test!` an anonymous
collection of inputs using a vector.

```clojure
(require '[conjtest.core :as conjtest])

(defn deny-integers-only
  [input]
  (when-not (every? integer? (get-in input [:foo :bar]))
    "all entries in :foo :bar should be integers"))

(conjtest/test! [{:foo {:bar [1 "2" 3]}}] deny-integers-only)
;; Execution error (ExceptionInfo) at conjtest.core/test-with-opts! (core.clj:284).
;; FAIL - null - all entries in :foo :bar should be integers
;; 
;; 1 tests, 0 passed, 0 warnings, 1 failures

(conjtest/test! [{:foo {:bar [1 2 3]}}] deny-integers-only)
;; => {:summary {:total 1, :passed 1, :warnings 0, :failures 0}, :summary-report "1 tests, 1 passed, 0 warnings, 0 failures\n", :result ({:message nil, :name nil, :rule-type :deny, :failure? false})}
```

See [tests](./test/conjtest/core_test.clj) for more usage examples.

For documentation on policies, see [Usage
chapter](https://user-guide.conjtest.org#usage) from Conjtest book.

For how to integrate with Babashka and
[ilmoraunio/conjtest](https://github.com/ilmoraunio/pod-ilmoraunio-conjtest)
using Babashka tasks, see
[example](https://github.com/ilmoraunio/conjtest/tree/main/demo/external_use).
