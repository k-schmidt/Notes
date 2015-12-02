# Structure and Interpretation of Computer Programs - Chapter 1.2
## Procedures and the Processes They Generate
* "To become experts, we must learn to visualize the processes generated by various types of procedures."(40)

## 1.2.1 Linear Recursion and Iteration
* Consider the factorial function

 `n! = n * (n-1) * (n-2) ...`

 `n! = n * (n-1)!`
* Then we can observe that this translates (*recursively*) to

```clojure
(defn factorial [n]
  (if (= n 1)
      n
      (* n (factorial (dec n)))))
```
* Iteratively
  * We could describe a rule for computing n! by maintaining a running product together with a counter that counts from 1 to n and changes from one step to the next.
  * product <- counter * product
  * counter <- counter + 1
  * n! is the product of the counter and the product when the counter exceeds n.

```clojure
(defn fact-iter [product counter max-count]
  (if (> counter max-count)
      product
      (fact-iter (* counter product) (inc counter) max-count)))

(defn factorial [n]
  (fact-iter 1 1 n))
```
Both the recursive and iteration methods look and behave similarly but one must consider their shapes.
* The recursive method expands and contracts, building up a chain of *deferred operations*.
  * Requires that the interpreter keep track of the operations to be performed later on.
* The iterative process does not grow or shrink.
  * State can be summarized by a fixed number of *state variables* together with a rule that describes how the state variables should be updated as the process moves from state to state.

Recursive Process versus Recursive Procedure
* Recursive Procedure
  * Refers to the syntactic fact that the procedure definition refers to the procedure itself.
* Recursive Process
  * Refers to how a process evolves, not about the syntax of how a procedure is written.

Tail Recursion
* Iteration can be expressed using the ordinary procedure call mechanism so that special iteration constructs are useful only as syntactic sugar.

Ex 1.9
```clojure
;; Recursive
(defn + [a b]
  (if (= a 0)
      b
      (inc (+ (dec a) b))))

;; Iterative
(defn + [a b]
  (if (= a 0)
      b
      (+ (dec a) (inc b))))

;; Evaluate (+ 4 5) Recursively
(inc (+ 3 5))
  (inc (+ 2 5))
    (inc (+ 1 5))
      (inc (+ 0 5))
        (5)
      (inc 5)
    (inc 6)
  (inc 7)
(inc 8)
9

;; Evaluate (+ 4 5) Iteratively
(+ 4 5)
(+ 3 6)
(+ 2 7)
(+ 1 8)
(+ 0 9)
9
```

Ex 1.10
Please see Scheme solution [here](http://community.schemewiki.org/?sicp-ex-1.10).