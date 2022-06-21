# Scheme notes

- [First item of a list](#first-item-of-a-list)
- [All items except the first](#All items except the first)
- [Prepend item to list](#Prepend item to list)
- [Print to terminal](#Print to terminal)
- [Open REPL](#Open REPL)
- [Run file](#Run file)
- [Compile](#Compile)
- [Apply a function to all elements of list](#Apply a function to all elements of list)
- [Apply a function for all the items in pairs](#Apply a function for all the items in pairs)
- [Check for empty list](#Check for empty list)
- [Combine strings](#Combine strings)
- [If statement](#If statement)
- [Do one operation after the other](#Do one operation after the other)
- [If statement with multiple operations](#If statement with multiple operations)
- [Check for equality](#Check for equality)
- [Assign variable](#Assign variable)
- [Define function](#Define function)
- [Hash tables/maps](#Hash tables/maps)
- [Create hash-map](#Create hash-map)
- [Pass list as arguments](#Pass list as arguments)

## Notes

* `tinyscheme` has some functions missing unless `init.scm` is added to the current directory. (\*) means that implementation needs `init.scm`.


## First item of a list

**Scheme**

```scheme
(car list)
```

**Clojure**

```clojure
(first list)
```

## All items except the first

**Scheme**

```scheme
(cdr list)
```

**Clojure**

```clojure
(rest list)
```

## Prepend item to list

**Scheme**

```scheme
(cons '(1 2 3 4))
```

**Clojure**

```clojure
(first [1 2 3 4])
```

## Print to terminal

**Scheme**

```scheme
(display "Hello, world!\n")
```

**Clojure**

```clojure
(println "Hello, world!\n")
```

## Open REPL

**Scheme**

```shell
chezscheme [FILE]
```

**Clojure**

```shell
lein repl
```

## Run file

**Scheme**

```shell
chezscheme --script [FILE]
```

**Clojure**

```shell
lein run
```

## Compile

**Scheme**

```shell
csc [FILE]
```

**Clojure**

```shell
lein uberjar
```

## Apply a function to all elements of list

**Scheme**

```scheme
(map add1 '(1 2 3 4))
; => (2 3 4 5)
```

**Clojure**

```clojure
(map inc [1 2 3 4])
; => [2 3 4 5]
```

## Apply a function for all the items in pairs

**Scheme**
```scheme
(define reduce
  (lambda (fn list)
	((null? list) '())))
 
(reduce + '(1 2 3 4 5))
;; => 15
```

**Clojure**

```clojure
(reduce + [1 2 3 4 5])
;; => 15
```

## Check for empty list

**Scheme**

```scheme
(null? list)
```

**Clojure**

```clojure
(empty? list)
```

## Combine strings

**Scheme**

```scheme
(string-append "hello" "world")
; => "helloworld"
```

**Clojure**

```clojure
(str "hello" "world")
; => "helloworld"
```

## If statement

**Scheme**

```scheme
(cond
 ((> 1 2) #t)
 (else #f))
```

or (\*)

```scheme
(if (> 1 2)
	#t
	#f)
```

**Clojure**

```clojure
(if (> 1 2)
	true
	false)
```

## Do one operation after the other

**Scheme**

```scheme
(begin
 (+ 1 2)
 (+ 2 3))
```

**Clojure**

```clojure
(do
 (+ 1 2)
 (+ 2 3))
```

## If statement with multiple operations

**Scheme**

```scheme
(when #t
	(+ 1 2)
	(+ 3 4))
```

**Clojure**

```clojure
(when true
	(+ 1 2)
	(+ 3 4))
```

## Check for equality

```scheme
(equal? 1 "Hello")
;; => #f
```

**Clojure**

```clojure
(= 1 "Hello")
;; => false
```

## Assign variable

```scheme
(define name "Michael")
name
; => Michael
```

```clojure
(def name "Michael")
name
; => Michael
```

## Define function

```scheme
(define add1
  (lambda (x)
	(+ 1 x)))

(add1 7)
; => 8
```

```clojure
(defn add1
	[x]
	(+ 1 x))

(add1 7)
; => 8
```

## Hash tables/maps

**Scheme**

R5RS doesn't really include hash tables (it was included in R6RS). Some Scheme implentations have them by adding SRFI 69 but the most basic Schemes don't have it. A (less efficient) alternative are (improper) association lists:

```scheme
(define person
  '((first-name . "John")
  (last-name . "Smith")))

person
; => (("first-name" . "John") ("last-name" . "Smith"))
```

**Clojure**

```clojure
(def person
  {:first-name "John"
  :last-name "Smith"})

person
; => {:first-name "John", :last-name "Smith"}
```

## Create hash-map

**Scheme**

Equivalent with association lists:

```scheme
(define alist
 (lambda args
   (cond
    ((null? args) '())
    (else
     (cons (cons (car args) (cadr args))
           (apply alist (cddr args)))))))

(alist 'a '1 'b '2)
```

**Clojure**

```clojure
(hash-map :a 1 :b 2)
```

## Pass list as arguments

**Scheme**

```scheme
(apply + '(1 2 3))
```

**Clojure**

```clojure
(apply + [1 2 3])
```

## Nested hash maps

**Scheme**

```scheme
'(name ((first . "John") (middle . "Jacob") (last . "Smith"))
```

**Clojure**

```clojure
“{:name {:first "John" :middle "Jacob" :last "Smith"}}”
```

## Create hash map accepting nested hash maps

**Scheme**

```scheme
(define alist
 (lambda args
   (cond
    ((null? args) '())
    ((list? (cadr args))
     (cons (cons (car args) (list (cadr args)))
           (apply alist (cddr args))))
    (else
     (cons (cons (car args) (cadr args))
           (apply alist (cddr args)))))))


(alist 'name '((first . "John") (middle . "Jacob") (last . "Smith")))
```

**Clojure**

```clojure
(hash-map :name {:first "John" :middle "Jacob" :last "Smith"})
```

## Equivalences

**Scheme\***

| If comparing...       | then use... |
| ---                   | ---         |
| numbers               | `=`         |
| non numeric           | `eqv?`       |
| strings               | `string=?`(*) |
| lists                 | `equal?`     |
| don't use             | `eq?`        |

##  Look up values

**Scheme**

```scheme
(define get
  (lambda (alist key)
    (cdr (assoc key alist))))

(get '((a . 0) (b . ((c . "ho hum")))) 'b)
; => ((c . ho hum))
```

technically, Scheme also accepts:

```scheme
(get '((:a . 0) (:b . ((:c . "ho hum")))) 'b)
```

but
- `'a` in Scheme is a symbol
- Clojure has symbols and keywords
	- `:a` in Clojure is a keyword, not a symbol

**Clojure**

```clojure
(get {:a 0 :b {:c "ho hum"}} :b)
; => {:c "ho hum"}
```

## Write to file

**Scheme(\*)**

```scheme
(call-with-output-file "out.txt"
  (lambda (p)
    (begin
      (write "Hello, world!" p)
      (newline p))))
```

## Check if file exists

**Scheme**

Not the most elegant but I think should work.

```scheme
(define file-exists?
  (lambda (file)
    (let ((port (open-input-file file)))
          (if (port? port)
            (close-input-port port)
            #f))))

(file-exists? "/bin/bash")
; => #t
(file-exists? "/bin/bosh")
; => #f
```

