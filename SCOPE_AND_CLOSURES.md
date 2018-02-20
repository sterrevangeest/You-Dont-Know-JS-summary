# You Don't Know JS: Scope & Closures
## Chapter 1: What is Scope? 

The inclusion of variables into our program begets the most interesting questions we will now address: where do those variables live? In other words, where are they stored? And, most importantly, how does our program find them when it needs them?

## Compiler Theory
Despite the fact that JavaScript falls under the general category of "dynamic" or "interpreted" languages, it is in fact a _compiled_ language. It is not compiled well in advance, as are many traditionally-compiled languages, nor are the results of compilation portable among various distributed systems.

In a traditional compiled-language process, a chunk of source code, your program, will undergo typically three steps _before_ it is executed, roughly called "compilation":

1. **Tokenizing/Lexing**: breaking up a string of characters into meaningful (to the language) chunks, called tokens. 
2. **Parsing**: taking a stream (array) of tokens and turning it into a tree of nested elements, which collectively represent the grammatical structure of the program. This tree is called an "AST" (Abstract Syntax Tree
3. **Code-Generation**: he process of taking an AST and turning it into executable code. This part varies greatly depending on the language, the platform it's targeting, etc.

The JavaScript engine is vastly more complex than just those three steps, as are most other language compilers. For instance, in the process of parsing and code-generation, there are certainly steps to optimize the performance of the execution, including collapsing redundant elements, etc.

Let's just say, for simplicity's sake, that any snippet of JavaScript has to be compiled before (usually right before!) it's executed. So, the JS compiler will take the program `var a = 2;` and compile it first, and then be ready to execute it, usually right away



## Understanding Scope
Think of terms of conversations:

The "characters" that interact to process the program `var a = 2`.

1. **The engine**: responsible for start-to-finish compilation execution of our JavaScript program
2. **Compiler**: one of _Engine's_ friends: handles all the dirty work of parsing and code-generation. 
3. **Scope**: another friend of _Engine_; collects and maintains a look-up list of all the declared identifiers (variables), and enforces a strict set of rules as to how these are accessible to currently executing code.

**Nested Scope**
Just as a block or function is nested inside another block or function, scopes are nested inside other scopes.
So, if a variable cannot be found in the immediate scope, _Engine_ consults the next outer containing scope, continuing until found or until the outermost (aka, global) scope has been reached.

The simple rules for traversing nested Scope: Engine starts at the currently executing Scope, looks for the variable there, then if not found, keeps going up one level, and so on. If the outermost global scope is reached, the search stops, whether it finds the variable or not.

### Back & Forth

Statement:
`var a = 2;`

But the Engine sees two distinct statements: 
- one which _compiler_ will handle during compilation
- one which _engine_ will handle during execution 

How _Engine and friends_ will approach the program `var a = 2`; 

1. The _compiler_ : perform lexing to break it down into tokens --> parse into a tree. 
2. The _compiler_ : code-generation. _Compiler_ will proceed as: 
  * Encountering `var a`, _Compiler_ asks _Scope_ to see if a variable `a` already exists for that particular scope collection. 
    - If so _Compiler_ ingnores this declaration and moves on. 
    - Otherwise, _Compiler_ asks _Scope_ to declare a new variable called `a` for that scope collection. 
  * _Compiler_ then produces code for _Engine_ to later execute, to handle the `a = 2` assignments. The code _Engine_ runs will first ask _Scope_ if there is a variable called `a` accessible in the current scope collection. 
    - If so _Engine_ uses that variable
    - If not, _Engine_ looks elsewhere. 
    
If _Engine_ eventually finds a variable, it assigns the value `2` to it. If not, _Engine_ will raise its hand and yell out an error!
To summarize: two distinct actions are taken for a variable assignment: 
First, Compiler declares a variable (if not previously declared in the current scope), and second, when executing, Engine looks up the variable in Scope and assigns to it, if found.

**Compiler Speak**

When _Engine_ executes the code that _Compiler_ produced step (2), it has to look-up the variable `a` to see if it has been declared, and this look-up is consulting _Scope_. 

In this case 

`var a = 2` 

_Engine_ would be performing an Left-hand Side (LHS) look-up for the variable `a`. The other type look-up is called Right-hand Side (RHS).(Side of an assignment operation. )

LHS = when a variable appears on the left-hand side of an assignement operation
RHS = when a variable appears on the right-hand of an assignent operation 

Ask yourself: _"who's the target of the assignment (LHS)"_ and _"who's the source of the assignment (RHS)"._

Why does it matter whether we call it LHS or RHS? Because these two types of look-ups behave differently in the circumstance where the variable has not yet been declared (is not found in any consulted Scope).
 

  
  
