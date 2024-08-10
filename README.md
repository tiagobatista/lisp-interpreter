# lisp-interpreter

Lisp is a general-purpose, multi-paradigm programming language suited for a wide variety of industry applications. It is probably most widely know for being the programming language built into Emacs. As well as that it was used to build Hacker News (Paul Graham is a Lisp fan), Grammarly, Circle CI (actually using the modern dialect Clojure) and Boeing.

In this Coding Challenge you are going to build your own simple Lisp interpreter. Like all the challenges, you can tackle it in any programming language you like. Bonus points if you go all Inception on it and build the interpreter in a modern version of Lisp, like Clojure or Racket!

If you want to learn a little Lisp (which is a fun thing to do), I pulled together some tutorial material in a LinkedIn post: 10 ways to learn Lisp. In the meantime here’s a very brief introduction.

There are two fundamental elements to Lisp syntax, atoms and s-expressions. 1, :cc and t are all atoms, where as (+ 1 2) and (defun hello () "Hello Coding Challenges") are s-expressions. In these examples 1 and 2 are integers, :cc is a symbol and t is an atom denoting true.

An s-expression is either an atom or an expression, for example (a b c). Where a, b, and c can all then be either atoms or s-expressions. Mathematical operations are in prefix notation.

Step Zero

Coding Challenges like the majority of common programming languages arrays is zero based, we start with Step 0! For this step please setup your IDE / editor of choice and programming language of choice.

Step 1

In this step your goal is to tokenise the expressions in a string. My personal approach to this would be to use test-driven development (TDD) to build tests for some example Lisp, i.e.:

1 this will give you the atom with a value of 1.
"Hello, Coding Challenges” This is a string atom.
:CC this should be an atom (which will evaluate to the symbol :CC).
(format t "Hello, Coding Challenge World World") this is an s-expression which should tokenise as a list comprising of a symbol followed by a list.
(defun hello () "Hello, Coding Challenge World").
Be sure to add your own test cases too.

Step 2

In this step your goal is to turn our tokenised string into an abstract syntax tree. By it’s nature, Lisp’s s-expressions can easily be turned into a binary tree. You could then represent that as an actual tree, or as a list, where some entries in the list are themselves lists.

For example consider the Lisp code: (defun doublen (n) (* n 2)) you could represent that as a tree or as the list: ['defun', 'doublen', ['n'], ['*', 'n', 2]].

Again, I’d suggest building some test cases. Here are some classic examples to use:

(defun fib (n)
  (if (< n 2)
      n
      (+ (fib (- n 1))
         (fib (- n 2)))))

(defun fact (n) 
  (if (<= n 1) 
    1 
    (* n (fact (- n 1)))))
Step 3

In this step your goal is to be able to evaluate an AST and execute it. To be able to do that you’ll need to have some way to look up the symbols that have been tokenised. For example when your evaluation function encounters the + operator it will need to know to evaluate and add together the two arguments.

To be able to do this your code will need to be able to look up the symbol in a mapping from variable name to value. This should include support for standard functions, like addition, subtraction and so on as well as user-defined variables.

You can create nested mappings to support local variables - look look up the variable in the innermost mapping then work outwards to access the global scope.

For this challenge I’d suggest you support the mathematical operators, if, defun, format and the comparison operators.

You goal for this step is to be able to run some simple code:


(+ 1 2)
(* 2 3)
(defun doublen (n) (* 2 n))
(doublen 4)  

Step 4

In this step your goal is to implement a REPL (Read-Evaluate-Print Loop). Essentially a prompt that allows the user to enter and expression and immediately see it read, evaluated and the output printed. Like Python if your familiar with it.

Once done you should be able to run the code above through it:

cclisp> (+ 1 2)
3
cclisp>  (* 2 3)
6
cclisp> (defun doublen (n) (* 2 n))
DOUBLEN
cclisp> (doublen 4)  
8
where cclisp> is the prompt for the Coding Challenges Lisp REPL.

Step 5

In this step your goal is to read and execute a Lisp script from file. Here’s an example script you could use:

(defun hello ()
  (format t "Hello Coding Challenge World~%"))

(defun doublen (n)
  (* n 2))

(defun fib (n)
  (if (< n 2)
      n
      (+ (fib (- n 1))
         (fib (- n 2)))))

(defun fact (n)
  (if (<= n 1)
    1
    (* n (fact (- n 1)))))

(hello)

(format t "The double of 5 is ~D~%" (doublen 5))

(format t "Factorial of 5 is ~D~%" (fact 5))

(format t "The 7th number of the Fibonacci sequence is ~D~%" (fib 7))
Which should give you:

Hello Coding Challenge World
The double of 5 is 10
Factorial of 5 is 120
The 7th number of the Fibonacci sequence is 13
