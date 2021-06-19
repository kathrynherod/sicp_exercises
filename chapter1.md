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
## Exercise 1.1: My solutions
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