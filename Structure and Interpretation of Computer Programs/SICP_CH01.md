# Structure and Interpretation of Computer Programs - Chapter 1
## Introduction by Alan J. Perlis
* A programmer should acquire good algorithms and idioms.
* It is better to have 100 functions operate on one data structure than to have 10 functions operate on 10 data structures.
* Programs must be written for people to read and only incidentally for machines to execute.
* John McCarthy - founder of Lisp in the 1950s and wrote "Recursive Functions of Symbolic Expressions and Their Computation by Machine".

## 1.1.0 The Elements of Programming
All programming languages must have:
* Primitive Expressions
 * Represent the simplest entities the language is concerned with.
* Means of Combination
 * By which compound elements are built from simpler ones.
* Means of Abstraction
 * By which elements can be named and manipulated as units.

Function's prefix notation has several advantages:
* Procedures can be an arbitrary number of elements

```clojure
  (+ 1 2 3 4 5 24)
```

* Allows combinations to be nested and to have combinations whose elements themselves are combinations.

```clojure
(+ (+ 1 2) (+ 3 4) (+ 5 6))
```

## 1.1.2 Naming and the Environment
* We say that the name identifies a variable whose value is the object.
* *def* is our language's simplest means of abstraction, for it allows us to use simple names to refer to the results of compound operations.

```clojure
(def radius 10)

(def pi 3.14159)

(def circumference
  (* 2 pi radius))
```

## 1.1.3 Evaluating Combinations
* Our goal is to isolate our issues about thinking procedurally.
 * The objective is to think in combinations.

To evaluate a combination:
1. Evaluate the subexpressions of the combination.
2. Apply the procedure that is the value of the leftmost subexpression (the operator) to the arguments that are the values of the other subexpressions (the operands).

Thereby we must first evaluate all inner expressions **first** before we can evaluate the entire expression.
We must evaluate each element of the combination. Thus, the evaluation rule is recursive in nature.
 * It includes, as one of its steps, the need to invoke the rule itself.

## 1.1.4 Compound Procedures
* Numbers and arithmetic operations are primitive data and procedures.
* Nesting of combinations provides a means of combining operations.
* Definitions that associate names with values provide a limited means of abstraction.

*Procedure Definitions* - compound operations can be given a name and then referred to as a unit.

*General Form of Procedural Definition*

`(defn <name> [<formal parameters>]
  <body>)`

`<name>` is a symbol to be associated with the procedure definition in the environment.

`<formal parameters>` are names used within the body of the procedure to refer to the corresponding arguments of the procedure.

`<body>` an expression that will yield the value of the procedure application when the formal parameters are replaced by the actual arguments to which the procedure is applied.

## 1.1.5 The Substitution Model for Procedure Application
* To apply a compound procedure to arguments, evaluate the body of the procedure with each formal parameter replaced by the corresponding argument.

Evaluate `(f 5)`
We define f to be:

```clojure
(defn square [x]
  (* x x))

(defn sum-of-squares [x y]
  (+ (square x) (square y)))

(defn f [a]
  (sum-of-squares (+ a 1) (* a 2)))

(sum-of-squares (+ 5 1) (* 5 2))

(+ (square 6) (square 10))

(+ (* 6 6) (* 10 10))

(+ 36 100)

136
```

* *Substitution Model* - model that determines the meaning of procedure applications.
 * Just a model to help us think about procedure applications, not to provide a description of how the interpreter really works.
* When modeling phenomena in science and engineering, we begin with simplified, incomplete models.
 * Simple models are transitioned to more refined ones.

Applicative Order Versus Normal Order
Inside-Out Versus Outside-In
* Normal order model is the evaluation method of fully expanding a procedure's body and then reducing to the answer.
 * First trace the program and expand all arguments and procedures to their fullest form.
* **However** the interpreter uses an *evaluate the arguments and then apply* approach called *Applicated-order Evaluation*.
* Lisp uses applicative-order evaluation because of efficiency of not duplicating expressions and also normal-order evaluation becomes much more complicated when we leave the realm of substitution.

## 1.1.6 Conditional Expressions and Predicates
Case analysis through `cond`

```clojure
(defn abs [x]
  (cond ((> x 0) x)
        ((= x 0) 0)
        ((< x 0) (- x))))
```

General Form

`(cond (<p1> <e1>)
       (<p2> <e2>) ...
       (<pn> <en>))`

Symbol cond followed by parenthesized pairs of expressions `(<p> <e>)` called clauses where `<p>` value is either True or False.
Predicates `<p1>...<pn>` are checked for 'truthiness' and takes the first truthy result that the procedure finds (consequent expression `<e>`). If none of the `<p>'s` are true then the value of cond is undefined.

Another way of computing the absolute value:

```clojure
(defn abs [x]
  (cond ((< x 0) (- x))
        (else x)))
```

*Else* is a special symbol that can be used in place of the `<p>` in the final clause of cond.

Yet another way...:

```clojure
(defn abs [x]
  (if (< x 0) (- x) x))
```

* Uses special form *if*, a restricted type of conditional that is used when there are precisely two cases in the case analysis.

General Form

`(if <predicate> <consequent> <alternative>)`

* The interpreter evaluates the `<predicate>`
 * If true, it evaluates the `<consequent>`
 * If false, it returns the evaluation of the `<alternative>`

* Clojure has primitive predicates <, >, =
 * Also logical composition operations to support construction of compound predicates.

`(and <e1> <e2> ... <en>)`
