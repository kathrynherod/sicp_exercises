# Chapter 1 Exercises

## Exercise 1.1:
Below is a sequence of expressions. What is the result printed by the interpreter in response to each expression? Assume that the sequence is to be evaluated in the order in which it is presented.


1. `10`
2. `(+ 5 3 4)`
3. `(- 9 1)`
4. `(/ 6 2)`
5. `(+ (* 2 4) (- 4 6))`
6. `(define a 3)`
7. `(define b (+ a 1))`
8. `(+ a b (* a b))`
9. `(= a b)`
10.
```
(if (and (> b a) (< b (* a b)))
    b
    a)
```
11.
```
(cond ((= a 4) 6)
      ((= b 4) (+ 6 7 a))
      (else 25))
```
12. `(+ 2 (if (> b a) b a))`
13.
```
(* (cond ((> a b) a)
         ((< a b) b)
         (else -1))
   (+ a 1))
```
### My Solutions
1. `10` -> 10
2. `(+ 5 3 4)` -> 12
3. `(- 9 1)` -> 8
4. `(/ 6 2)` -> 3
5. `(+ (* 2 4) (- 4 6))` -> 6
6. `(define a 3)` -> a = 3
7. `(define b (+ a 1))` -> b = 4
8. `(+ a b (* a b))` -> 19
9. `(= a b)` -> false?
10.
```
(if (and (> b a) (< b (* a b)))
    b
    a)
```
```
(if (and (> 4 3) (< 4 (* 3 4))) <predicate> -> (and(true)(4<12)) -> (and(true)(true)) -> true
    b <consequent> predicate true then do consequent
    a) <alternative> predicate false then do alternative
    -> 4
```
11.
```
(cond ((= a 4) 6)
      ((= b 4) (+ 6 7 a))
      (else 25))
```
```
(cond ((= a 4) 6) false
      ((= b 4) (true) (+ 6 7 a)) -> 6+7+3 -> 16
      (else 25))
      -> 16
```
12. `(+ 2 (if (> b a) b a))` -> 6
```
(+ 2 (if (> b a) b a))
(+ 2 (if (4 > 3) 4 3))
(+ 2 (true) -> 4) -> 6
```
13.
```
(* (cond ((> a b) a)
         ((< a b) b)
         (else -1))
   (+ a 1))
```
```
(* (cond ((> a b) a) false
         ((< a b) b) true -> 4
         (else -1))
   (+ a 1))

(* 4 (+ a 1))
(4 * 4) -> 16
-> 16
```

## Exercise 1.2:
Translate the following expression into prefix form:
```
5 + 4 + (2 − (3 − (6 + 4/5)))
____________________________
      3(6 − 2)(2 − 7)
```
### My Solution
```
;;;start with nested expressions
(6 + 4/5) -> (+ 6 (/ 4 5))
(3 − (6 + 4/5)) -> (- 3 (+ 6 (/ 4 5)))
(2 − (3 − (6 + 4/5))) -> (- 2 (- 3 (+ 6 (/ 4 5))))

(/ (+ 5 4
        (- 2
           (- 3
              (+ 6
                 (/ 4 5)))))
   (* 3
      (- 6 2)
      (- 2 7))
)

;;; answer
(/ (+ 5 4 (- 2 (- 3 (+ 6 (/ 4 5))))) (* 3 (- 6 2) (- 2 7)))
```
## Exercise 1.3:
Define a procedure that takes three numbers as arguments and returns the sum of the squares of the two larger numbers.
### My Solution
```
;;; find larger of first two nums
(define (max x y)
    (if (> x y) x y))
;;; find smaller of first two nums
(define (min x y)
    (if (< x y) x y))
;;; after finding larger number of first two numbers
;;; take the smaller one and compare against third num
(define (second-large-num x y z)
    (max z (min x y)))
(define (square x)
    (* x x))
(define (sum-of-squares x y)
    (+ (square x) (square y)))
(define (sum-of-squares-of-largest-nums n1 n2 n3)
    (sum-of-squares
        (max n1 n2) ;returns larger num of first two num params
        (second-large-num n1 n2 n3))) ;returns larger num of third param and smaller of first two params
```
## Exercise 1.4:
Observe that our model of evaluation allows for combinations whose operators are compound expressions. Use this observation to describe the behavior of the following procedure:
```
(define (a-plus-abs-b a b)
  ((if (> b 0) + -) a b))
```
### My Solution
`(if (> b 0) + -)` returns the operand for a primitive procedure to follow. When b is larger than 0, return `+`, otherwise, return `-`

`((operand returned from conditional) a b)` based on what the conditional returns, the numbers will either be added to each other, or `b` will be subtracted from `a`, thus creating the primitive procedure.

## Exercise 1.5:
Ben Bitdiddle has invented a test to determine whether the interpreter he is faced with is using applicative-order evaluation or normal-order evaluation. He defines the following two procedures:

`(define (p) (p))`
```
(define (test x y)
  (if (= x 0)
      0
      y))
```
Then he evaluates the expression

`(test 0 (p))`

What behavior will Ben observe with an interpreter that uses applicative-order evaluation? What behavior will he observe with an interpreter that uses normal-order evaluation? Explain your answer. (Assume that the evaluation rule for the special form if is the same whether the interpreter is using normal or applicative order: The predicate expression is evaluated first, and the result determines whether to evaluate the consequent or the alternative expression.)

### My Solution
I'll admit I had to look at the answer for this one to truly understand the interpreter evaluation order/behavior because my eyes glazed over a bit in the text when I first read it. This is in Section 1.1.5 The Substitution Model for Procedure Application: Applicative order versus normal order

- What behavior will Ben observe with an interpreter that uses applicative-order evaluation?

The first line that will be evaluated by the interpreter using applicative-order evaluation will be `(test 0 (p))`, and so it will look for the definition of `test` and its arguments. As it evaluates `test`, it will return the body of the procedure, but with `(p)`, it evaluates to a recursive call of itself and times out.

- What behavior will he observe with an interpreter that uses normal-order evaluation?

>If the interpreter uses normal-order evaluation, the (p) operand will not be evaluated until its value is needed. This is because when using normal-order evaluation the interpreter will substitute operand expressions for parameters. Since the conditional statement in the body is structured such that the second argument never needs to be evaluated, the entire test procedure will evaluate to 0 under normal-order evaluation." [Source](https://billthelizard.blogspot.com/2009/10/sicp-exercises-11-15.html)
