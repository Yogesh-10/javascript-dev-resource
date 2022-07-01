## 1. EXECUTION CONTEXT

Created to run the code of a function - has 2 parts

- Thread of execution
- Memory(saves data)

Everything in JS happens inside the execution context. Imagine a closed container inside which JS runs. It is an abstract concept that hold info about the env. within the context, current code is being executed.

- In the container the first component is **memory component** and the 2nd one is **code component**
- Memory component has all the variables and functions in key value pairs. It is also called **Variable environment**.
- Code component is the place where code is executed one line at a time. It is also called as **Thread of Execution**.
- JS is a **synchronous**, **single-threaded** language
    - Synchronous:- One command at a time.
    - Single-threaded:- In a specific synchronous order.

```jsx
const x = 10; //will be undefined in memory creation phase and 10 will be assigned during code is executed

function calculateVal(a, b){
	const res = a * b;
	return res;
}
```
![image](https://user-images.githubusercontent.com/71348279/164771514-dc6245ac-c49a-4bc1-a5de-917df22334cc.png)
<br>
<br>
<br>

## 2. HOW JS IS EXECUTED AND CALL STACK

- **Memory creation phase :** The place where all the variables and functions are stored as (key: value) pairs. For variable(s), key is the variable name itself and value is undefined (even if the variable is initialized). And, for function(s), key is the function name and value is body of the
code. Memory component is also known as ***`variable environment*.`**
- **Code execution phase:** The place where code is executed line by line at a time. Code component is also known as ***`Thread of Execution`.*** A new ‘local’ execution context is created, when function ‘invocation’ is encountered. Again, two phase perform their role.
- Javascript manages code execution context creation and deletion with the the help of **Call Stack**.
- Call Stack is a mechanism to keep track of its place in script that calls multiple function.
- Call Stack maintains the order of execution of execution contexts. It is also known as Program Stack, Control Stack, Runtime stack, Machine Stack, Execution context stack.

JavaScript keeps track of what function is currently running (where’s the thread of execution)

- Run a function - add to call stack
- Finish running the function - JS removes it from call stack
- Whatever is top of the call stack that’s the function we’re
currently running

> **Call Stack maintains the order of execution of execution contexts**
>

Call stack, with Global execution context at the bottom and all the subsequent function invocation or new execution context is pushed to this call stack. Once the execution context is done with the task, it is popped. Well, call stack is also called by names like, Execution context, Program, Machine, Control, Runtime stack

Every time you run a program, an execution context is created. When a variable or function is encountered, it is stored in the memory area.

```jsx
const num = 3;
function multiplyBy2 (inputNumber){
const result = inputNumber*2;
return result;
}
const output = multiplyBy2(num);
const newOutput = multiplyBy2(10);
```

Now first, for this entire code a **Global** execution context is created.

**In the first phase (memory creation)**

- Memory is allocated to variables and functions.
- For variable name(which is key) it assigns a value of **undefined**
- For the function name(which is key) it assigns the entire function code as value.

```jsx
num:undefined
multiplyBy2: {...function...}
output:undefined
newOutput:undefined
```

**In the second phase (code execution)**

- The variable name is replaced with its actual assigned value from code. So now num: 3
- Skips over function code as there is nothing to assign there.
- We encounter a function call in output. So a brand new local EC is created inside the code part of global EC and this will have the same 2 components: Memory and Code.
- In the local EC, inputNumber and result are both undefined (in first phase). Then, the num value in global EC is passed to inputNumber, replacing undefined. inputNumber is the parameter and num is the argument.
- result = inputNumber*2 (calculated in code part of local EC and returned) replaces undefined in local EC (memory part) and the final value is returned from local and is assigned to output in global. After returning, local EC is removed form global EC(popped off from call stack) and control goes back to global.
- same thing happens with newOutput

To manage all these EC, a call **stack** is created. Every time code is run, the EC is pushed in. So first global EC is pushed. Then multiplyBy2 EC(for output ) is pushed, and then after value returned, is popped. Similarly multiplyBy2 EC(for newOutput) is pushed, and then popped and finally Global is also popped and stack is empty.

![image](https://user-images.githubusercontent.com/71348279/164771821-298fe1b8-1c74-4e2c-9add-f7112894cd97.png)

<br>
<br>
<br>

## 3. HOISTING IN JAVASCRIPT(variables & functions)

It is the process of assigning a variable declaration a default value of undefined during the memory creation phase

Phenomena in JS through which we can access the variables and functions even before they have initialized, without any error.

Variable and class *declarations* are also hoisted, so they too can be referenced before they are declared. Note that doing so can lead to unexpected errors, and is not generally recommended.

```jsx
getName();
console.log(x);
var x = 7;
function getName(){
    console.log("Namaste JavaScript");
}

Output:
Namaste JavaScript
undefined
```

- It should have been an outright error in many other languages, as it is not possible to even access something which is not even created (defined) yet But in JS, We know that in memory creation phase it assigns undefined and puts the content of function to function's memory. And in execution, it then executes whatever is asked. Here, as execution goes line by line and not after compiling, it could only print undefined and nothing else. This phenomenon, is not an error. However, if we remove var x = 7; then it gives error. Uncaught ReferenceError: x is not defined
- **Hoisting** is a concept which enables us to extract values of variables and functions even before initializing/assigning value without getting error and this is happening due to the 1st phase (memory creation phase) of the Execution Context.
- So in previous topic, we learnt that execution context gets created in two phase, so even before code execution, memory is created so in case of variable, it will be initialized as undefined while in case of function the whole function code is placed in the memory. Example:

```jsx
getName();
console.log(x);

function getName(){
    console.log("Namaste JavaScript");
}

Output:
Namaste JavaScript
Error: x is not defined //not defined and "undefined" are totally different
```

- Not defined: We have not initialized the value for variable anywhere in the entire code and in memory space.
- Undefined: It is a placeholder that is assigned to a variable by the Javascript Engine until the variable is assigned with some other value.

**Hoisting** is a concept which enables us to extract values of variables and functions even before initializing/assigning value without getting *error*

```jsx
console.log(getName)

var getName = function () {
    console.log("Namaste JavaScript");
}

var getName = () => {
    console.log("Namaste JavaScript");
}

output: undefined //because they behave as variable and not function.
```

Reason for hoisting:

- The answer lies in the Global Execution Context. In the memory phase, the variables will be initialized as *undefined* and functions will get the whole function code in their memory.
- This is the reason why we are getting these outputs.

<br>
<br>
<br>


## 4. FUNCTIONS AND VARIABLE ENVIRONMENTS

```jsx
var x = 1;
a();
b(); // we are calling the functions before defining them. This will work properly, as seen in Hoisting.
console.log(x);

function a() {
  var x = 10; // local scope because of separate execution context
  console.log(x);
}

function b() {
  var x = 100;
  console.log(x);
}
```

**Code Flow in terms of Execution Context**

- The Global Execution Context (GEC) is created (the big box with Memory and Code subparts). Also GEC is pushed into Call Stack

> Call Stack : GEC
>
- In first phase of GEC (memory phase), variable x:undefined and a and b have their entire function code as value initialized
- In second phase of GEC (execution phase), when the function is called, a new local Execution Context is created. After x = 1 assigned to GEC x, a() is called. So local EC for a is made inside code part of GEC.

> Call Stack: [GEC, a()]
>
- For local EC, a totally different x variable assigned undefined(x inside a()) in phase 1 , and in phase 2 it is assigned 10 and printed in console log. After printing, no more commands to run, so a() local EC is removed from both GEC and from Call stack

> Call Stack: GEC
>
- Cursor goes back to b() function call. Same steps repeat.

> Call Stack :[GEC, b()] -> GEC (after printing yet another totally different x value as 100 in console log)
>
- Finally GEC is deleted and also removed from call stack. Program ends.

<br>
<br>
<br>


## 5. SHORTEST JS PROGRAM, WINDOW AND THIS KEYWORD

- The shortest JS program is empty file. Because even then, JS engine does a lot of things. As always, even in this case, it creates the GEC which has memory space and the execution context.
- JS engine creates something known as '**window**'. It is an object, which is created in the global space. It contains lots of functions and variables. These functions and variables can be accessed from anywhere in the program. JS engine also creates a **this** keyword, which points to the **window object** at the global level. So, in summary, along with GEC, a global object (window) and a this variable are created.
- In different engines, the name of global object changes. Window in browsers, but in nodeJS it is called something else. At global level, this === window
- If we create any variable in the global scope, then the variables get attached to the global object.

```jsx
var x = 10;
console.log(x); // 10
console.log(this.x); // 10
console.log(window.x); // 10
```

<br>
<br>
<br>


## 6. UNDEFINED VS NOTDEFINED IN JS

- In first phase (memory allocation) JS assigns each variable a placeholder called **undefined**.
- **undefined** is when memory is allocated for the variable, but no value is assigned yet.
- If an object/variable is not even declared/found in memory allocation phase, and tried to access it then it is **Not defined**
- Not Defined !== Undefined

> When variable is declared but not assigned value, its current value is undefined. But when the variable itself is not declared but called in code, then it is not defined.
>

```jsx
console.log(x); // undefined
var x = 25;
console.log(x); // 25
console.log(a); // Uncaught ReferenceError: a is not defined
```

- JS is a **loosely typed / weakly typed** language. It doesn't attach variables to any datatype. We can say *var a = 5*, and then change the value to boolean *a = true* or string *a = 'hello'* later on.
- **Never** assign *undefined* to a variable manually. Let it happen on it's own accord.

<br>
<br>
<br>


## 7. THE SCOPE, SCOPE CHAIN AND LEXICAL ENVIRIONMENT

**Lexical Scope -**  When a function is defined, it gets a bond to the surrounding Local Memory
(“Variable Environment”) in which it has been defined

The word *lexical* refers to the fact that lexical scoping uses the location where a variable is declared within the source code to determine where that variable is available. Nested functions have access to variables declared in their outer scope.

- **Scope is directly dependent on the lexical environment**
- **Lexical Environment** : local memory + lexical env of its parent
- Whenever an EC is created, a Lexical environment(LE) is also created and is referenced in the local EC(in memory space)

### **scope and scope chain examples**:

```jsx
// CASE 1
function a() {
    console.log(b); // 10
    // Instead of printing undefined it prints 10, So somehow this a function could access the variable b outside the function scope.
}
var b = 10;
a();
```

```jsx
// CASE 2
function a() {
    c();
    function c() {
        console.log(b); // 10
    }
}
var b = 10;
a();
```

```jsx
// CASE 3
function a() {
    c();
    function c() {
        var b = 100;
        console.log(b); // 100
    }
}
var b = 10;
a();
```

```jsx
// CASE 4
function a() {
    var b = 10;
    c();
    function c() {
        console.log(b); // 10
    }
}
a();
console.log(b); // Error, Not Defined
```

- Let's try to understand the output in each of the cases above.
    - In **case 1**: function a is able to access variable b from Global scope.
    - In **case 2**: 10 is printed. It means that within nested function too, the global scope variable can be accessed.
    - In **case 3**: 100 is printed meaning local variable of the same name took precedence over a global variable.
    - In **case 4**: A function can access a global variable, but the global execution context can't access any local variable.

        ```jsx
        To summarize the above points in terms of execution context:
        call_stack = [GEC, a(), c()]
        Now lets also assign the memory sections of each execution context in call_stack.
        c() = [[lexical environment pointer pointing to a()]]
        a() = [b:10, c:{}, [lexical environment pointer pointing to GEC]]
        GEC =  [a:{},[lexical_environment pointer pointing to null]]
        ```


Lexical env - comprises of local memory and ‘reference’ (and not ‘copy’) of lexical environment of its parent. For Global Exec. Context, lexical environment points to null.

```jsx
function a() {
    c();
		var b = 10;
    function c() {
        console.log(b);
    }
}
a();
```

![image](https://user-images.githubusercontent.com/71348279/164773041-9afe8916-b28b-4900-ab69-d1bbd24efdb8.png)

Whenever a variable is encountered while execution, it is first searched in the local memory space and if not found, it goes to the lexical environment of its parent and perform the same step, until found or reaches global scope.

As seen in above diagram, here first we have global execution context, and we have execution context for a( ) and we have execution context for b( ), for a( ) - lexical environment is it’s local memory + lexical environment of parent i.e, global execution context, and for b( ) → lexical environment is it’s local memory + lexical environment of parent i.e, a( ) + global execution context.

<br>
<br>
<br>


## 8. LET AND CONST IN JS, TEMPORAL DEAD ZONE, TYPES OF ERRORS

- let & const are hoisted but stored in different memory space than other variables like var. (And hence they cannot be access via window object or this specifier). They cannot be accessed until they’re initialized.
- Hence, the time from hoisting these variable(s) and initialization is temporal dead zone, and during this we cannot access let & const, in turns throws Reference error.
let cannot be re-declared in the same scope unlike var, it will throw Syntax Error.
- In case of const we need to declare and initialize at the same line/time.
If we try changing the value later at some line to const variable, we’ll get Type Error.
- let and const declarations are hoisted. But its different from **var**

```jsx
console.log(a); // ReferenceError: Cannot access 'a' before initialization
console.log(b); // prints undefined as expected
let a = 10;
console.log(a); // 10
var b = 15;
console.log(window.a); // undefined
console.log(window.b); // 15
```

It looks like let isn't hoisted, **but it is**, let's understand

- Both a and b are actually initialized as *undefined* in hoisting stage. But var **b** is inside the storage space of GLOBAL, and **a** is in a separate memory object called **script**, where it can be accessed only after assigning some value to it first ie. one can access 'a' only if it is assigned. Thus, it throws error.
- **Temporal Dead Zone** : Time since when the let variable was hoisted until it is initialized some value.
    - So any line till before "let a = 10" is the TDZ for a
    - Since a is not accessible on global, its not accessible in *window/this* also. window.b or this.b -> 15; But window.a or this.a ->undefined, just like window.x->undefined (x isn't declared anywhere)
- **Reference Error** are thrown when variables are in temporal dead zone.
- **Syntax Error** doesn't even let us run single line of code.

```jsx
let a = 10;
let a = 100;  //this code is rejected upfront as SyntaxError. (duplicate declaration)
------------------
let a = 10;
var a = 100; // this code also rejected upfront as SyntaxError. (can't use same name in same scope)
```

- **Let** is a stricter version of **var**. Now, **const** is even more stricter than **let**.

```jsx
let a;
a = 10;
console.log(a) // 10. Note declaration and assigning of a is in different lines.
____________________
const b;
b = 10;
console.log(b); // SyntaxError: Missing initializer in const declaration. (This type of declaration won't work with const. const b = 10 only will work)
____________________
const b = 100;
b = 1000; //this gives us TypeError: Assignment to constant variable.
```

- Types of **Error**: Syntax, Reference, and Type.
    - Uncaught ReferenceError: x is not defined at ...
        - This Error signifies that x has never been in the scope of the program. This literally means that x was never defined/declared and is being tried to be accesed.
    - Uncaught ReferenceError: cannot access 'a' before initialization
        - This Error signifies that 'a' cannot be accessed because it is declared as 'let' and since it is not assigned a value, it is its Temporal Dead Zone. Thus, this error occurs.
    - Uncaught SyntaxError: Identifier 'a' has already been declared
        - This Error signifies that we are redeclaring a variable that is 'let' declared. No execution will take place.
    - Uncaught SyntaxError: Missing initializer in const declaration
        - This Error signifies that we haven't initialized or assigned value to a const declaration.
    - Uncaught TypeError: Assignment to constant variable
        - This Error signifies that we are reassigning to a const variable.

        **Note**: In case of const array or object, if we try to change the value, it is
        perfectly fine/valid. The property of a const object can be change but it
        cannot be change to reference to the new object.

        **BEST PRACTICES:**

    - Try using const wherever possible.
    - If not, use let, Avoid var.
    - Declare and initialize all variables with let to the top to avoid errors to shrink temporal dead zone window to zero.

<br>
<br>
<br>


## 9. BLOCK SCOPE AND SHADOWING IN JS

- Block combines multiple js statement and can be used at places where single line is expected.
- let & const cannot be accessed outside the block, in which they reside. Hence, they’re called as block scoped, and var to be function scoped.

**What is a block?**

- Block aka *compound statement* is used to group JS statements together into 1 group. We group them within { }
- The purpose is to group multiple statements at a place where JS expects only 1 statement.

```jsx
//code example 1

if(true)some statement
```

But if we want to write more statements to execute after if condition; then:

```jsx
//code example 2

if(true){
    statement 1
    statement 2
    ...
}
```

- The { } block treats all the statements as one statement.
- The if doesn’t have any curly braces in syntax.

**BLOCK SCOPE**

- What are the variables and functions that can be accessed inside the block.

```jsx
//code example 3

{
    var a = 10;
    let b = 20;
    const c = 30;
}

console.log(a);
console.log(b);

/* Output:
10
Uncaught ReferenceError: b is not defined
*/
```

- Behind the Scenes:
    - In the BLOCK SCOPE; we get b and c inside it initialized as *undefined* as a part of hoisting (in a seperate memory space called block)
    - While, a is stored inside a GLOBAL scope.
- Thus we say, *let* and *const* are BLOCK SCOPED. They are stored in a separate mem space which is reserved for this block. Also, they can't be accessed outside this block. But var a can be accessed anywhere as it is in global scope.
- Thus, we can't access them outside the Block.

**What is SHADOWING in JS?**

```jsx
//code example 4

var a = 100;
{
    var a = 10; //same name as global var
    let b = 20;
    const c = 30;
    console.log(a); // 10
    console.log(b); // 20
    console.log(c); // 30
}

console.log(a); // 10 instead of the 100 we were expecting. So block "a" modified val of global "a" as well.
// In console, only b and c are in block space. a initially is in global space(a = 100), and when a = 10 line is run, a is not created in block space, but replaces 100 with 10 in global space itself.
```

- If one has same named variable outside the block, the variable inside the block *shadows* the outside variable.
- So, a is reassigned to 10. Since both refers to same memory space i.e GLOBAL SPACE. **This happens only for var**

**Instead of var let’s use let**

```jsx
//code example 5

let b = 100;
{
    var a = 10;
    let b = 20;
    const c = 30;
    console.log(b);
}

console.log(b);

/* 
20 
100 // this was what we were expecting in this first place. Both b's are in separate spaces (one in Block(20) and one in Script(another arbitrary mem space)(100))
```

Outputs:

- In the Scope, we have b in two places. The b outside of the block is in *Script* SCOPE (seperate memory space for let and const)
- The b inside the block is in *Block* scope.
- Thus, when in Block scope, 20 is printed and 100 when outside.
- This concept is called "Shadowing".
- It is also true for *const* declarations.

**Same logic is true even for functions**

```jsx
const c = 100;
function x() {
  const c = 10;
  console.log(c);
}
x();
console.log(c);
/*Output:
10
100
*/
```

**What is Illegal Shadowing?**

```jsx
// code example 6

let a = 20;
{
    var a = 20;
}

//Uncaught SyntaxError: Identifier 'a' has already been declared
```

- We cannot shadow let with var. But it is valid to shadow a let using a let.
- However, we can shadow var with let.
- All scope rules that work in function are same in arrow functions too.
- Since var is function scoped, it is not a problem with the code below.

```jsx
// code example 7

let a = 20;
function x() {
    var a = 20;
}
```

<br>
<br>
<br>


## 10. CLOSURES:

Closure is the most misunderstood of JavaScript concepts

- Enables powerful pro-level functions like ‘once’ and ‘memoize’
- Many JavaScript design patterns including the module pattern use closure
- Build iterators, handle partial application and maintain state in an
asynchronous world

Behavior of a function - Functions get a new memory every run/invocation

```jsx
function multiplyBy2 (inputNumber){
const result = inputNumber*2;
return result;
}
const output = multiplyBy2(7);
const newOutput = multiplyBy2(10);
```

multiplyBy2 is invoked twice, and both run in different execution contexts and separate from each other.

**Function with memories(closures):**

A **closure** is the combination of a function bundled together (enclosed) with references to its surrounding state (the **lexical environment**). In other words, a closure gives you access to an outer function's scope from an inner function. In JavaScript, closures are created every time a function is created, at function creation time.

Functions with memories

- When our functions get called, we create a live store of data (local memory/variable environment/state) for that function’s execution context.
- When the function finishes executing, its local memory is deleted (except the returned value)
- But what if our functions could hold on to live data between executions?
- This would let our function definitions have an associated cache/persistent memory
- But it all starts with us returning a function from another function

Functions can be returned from another functions in javascript, since function in js are first class citizens

```jsx
function createFunction() {
 function multiplyBy2 (num){
 return num*2;
 }
 return multiplyBy2;
}
const generatedFunc = createFunction();
const result = generatedFunc(3); // 6
```

Calling a function outside of the function call in which it was defined

```jsx
function outer (){
	 let counter = 0;
	 function incrementCounter (){ 
			counter ++; 
	 }
	 return incrementCounter;
}

const myNewFunction = outer();
myNewFunction();
myNewFunction();
```

**Lexical Scope -**  When a function is defined, it gets a bond to the surrounding Local Memory
(“Variable Environment”) in which it has been defined

The word *lexical* refers to the fact that lexical scoping uses the location where a variable is declared within the source code to determine where that variable is available. Nested functions have access to variables declared in their outer scope.

- Scope is directly dependent on the lexical environment
- **Lexical Environment** : local memory + lexical env of its parent
- Whenever an EC is created, a Lexical environment(LE) is also created and is referenced in the local EC(in memory space)

It comprises of local memory and ‘reference’ (and not ‘copy’) of lexical environment of its parent. For Global Exec. Context, lexical environment points to null.

```jsx
function a() {
    c();
		var b = 10;
    function c() {
        console.log(b);
    }
}
a();
```

Diagram for above code,

![image](https://user-images.githubusercontent.com/71348279/164924955-9819c912-08db-4d84-aa35-69750e9dc4c3.png)

Whenever a variable is encountered while execution, it is first searched in the local memory space and if not found, it goes to the lexical environment of its parent and perform the same step, until found or reaches global scope.

As seen in above diagram, here first we have global execution context, and we have execution context for a( ) and we have execution context for b( ), for a( ) - lexical environment is it’s local memory + lexical environment of parent i.e, global execution context, and for b( ) → lexical environment is it’s local memory + lexical environment of parent i.e, a( ) + global execution context.

**Closure code explanation**

```jsx
function outer (){
	 let counter = 0;
	 function incrementCounter (){ 
			counter ++; 
	 }
	 return incrementCounter;
}

const myNewFunction = outer();
myNewFunction(); //1
myNewFunction(); //2

```

when we run this code, outer( ) is called, the local memory of outer has counter and incrementCounter function, and we return incrementCounter function from outer. now the result of calling outer is stored in myNewFunction which is function body of incrementCounter.

Now that, outer has finished execution and removed from callstack. Now we have myNewFunction which is function definition of incrementCounter, so we can invoke it. But when we invoke myNewFunction( ), the function body has counter++, but we don’t have access to counter inside incrementCounter. But javascript stores the function body along with variables in lexical scope of that function, what this means is js stores whole function body of incrementCounter in to myNewFunction, and along with it there is an hidden property [[scope]] which contains variables in lexical environment, in this example - counter. we can’t access the value of counter because it is a hidden property, we can get result of counter only by running the myNewFunction( ). 

step 1: when we invoke myNewFunction( ), a brand new execution context is created and when code runs, there is no counter in local memory, so we check it’s parent which is global scope, there we have newFunction variable is in global scope, which has function body along with variables in lexical environment. counter is 0 and now increased to 1.

So newFunction variable is in global scope, which has function body along with variables in lexical environment, so when myNewFunction( ) is invoked again there is no local memory inside execution context of myNewFunction( ) so it looks in global memory where counter is 1, which is increased by 1 and now value of counter is 2

![image](https://user-images.githubusercontent.com/71348279/164923614-42309bae-0626-477c-b564-0a8d34a9957e.png)

The hidden property [[scope]] we can refer them as ‘backpack’ since they carry extra variables in it’s lexical scope along with function definition

The ‘backpack’ (or ‘closure’) of live data is attached incrementCounter (then to
myNewFunction) through a hidden property known as [[scope]] which persists
when the inner function is returned out

The ‘backpack’ -

We return incrementCounter’s code (function definition) out of outer into global and give it a new name - myNewFunction

- We maintain the bond to outer’s live local memory - it gets ‘returned out’ attached on the back of incrementCounter’s function definition.
- So outer’s local memory is now stored attached to myNewFunction - even though outer’s execution context is long gone
- When we run myNewFunction in global, it will first look in its own local memory first (as we’d expect), but then in myNewFunction’s ‘backpack’

Technical terms for ‘backpack’

- Closed over ‘Variable Environment’ (C.O.V.E.)
- Persistent Lexical Scope Referenced Data (P.L.S.R.D.)
- ‘Backpack’
- ‘Closure’

Example 2

```jsx
function outer (){
 let counter = 0;
 function incrementCounter (){
	 counter ++;
 }
 return incrementCounter;
}

const myNewFunction = outer();
myNewFunction(); //1
myNewFunction(); //2

//Let’s run outer again
const anotherFunction = outer();
anotherFunction(); //1
anotherFunction(); //2
```

Both maintain different execution context as outer creates new execution context, which is completely different from myNewFunction  and anotherFunction. so we get individual backpack to each function call for outer( )

If we run 'outer' again and store the returned 'incrementCounter' function
definition in 'anotherFunction', this new incrementCounter function was created in
a new execution context and therefore has a brand new independent backpack

**Closure  use cases**

Closure gives our functions persistent memories and entirely new toolkit for writing professional code

**Helper functions**: Everyday professional helper functions like ‘once’ and ‘memoize’
**Iterators and generators**: Which use lexical scoping and closure to achieve the most contemporary patterns for handling data in JavaScript
**Module pattern**: Preserve state for the life of an application without polluting the global namespace
**Asynchronous JavaScript**: Callbacks and Promises rely on closure to persist state in an asynchronous environment

- **Advantages of Closure:**
    - Module Design Pattern
    - Currying
    - Memoize
    - Data hiding and encapsulation
    - setTimeouts etc.
- **Disadvantages of Closure:**
    - Over consumption of memory
    - Memory Leak
    - Freeze browser

**Garbage Collector**
It is a program in the browser/JS Engine which is responsible for freeing
up the memory which are unused.

<br>
<br>
<br>


## 11. CLOSURE INTERVIEW QUESTION + SETTIMEOUT

```jsx
function x() {
    var i = 1;
    setTimeout(function() {
        console.log(i);
    }, 3000);
    console.log("Namaste Javascript");
}
x();
// Output:
// Namaste Javascript
// 1 // after waiting 3 seconds
```

- We expect JS to wait 3 sec, print 1 and then go down and print the string. But JS prints string immediately, waits 3 sec and then prints 1.
- The function inside setTimeout forms a closure (remembers reference to i). So wherever function goes it carries this ref along with it.
- setTimeout takes this callback function & attaches timer of 3000ms and stores it. Goes to next line without waiting and prints string.
- After 3000ms runs out, JS takes function, puts it into call stack and runs it.
- Q: Print 1 after 1 sec, 2 after 2 sec till 5 : Tricky interview question
    
    We assume this has a simple approach as below
    
    ```jsx
    function x() {
    for(var i = 1; i<=5; i++){
        setTimeout(function() {
        console.log(i);
        }, i*1000);
        }
        console.log("Namaste Javascript");
    }
    x();
    // Output:
    // Namaste Javascript
    // 6
    // 6
    // 6
    // 6
    // 6
    ```
    
    - Reason?
        - This happens because of closures. When setTimeout stores the function somewhere and attaches timer to it, the function remembers its reference to i, **not value of i**. All 5 copies of function point to same reference of i. JS stores these 5 functions, prints string and then comes back to the functions. By then the timer has run fully. And due to looping, the i value became 6. And when the callback fun runs the variable i = 6. So same 6 is printed in each log
        - To avoid this, we can use **let** instead of **var** as let has Block scope. For each iteration, the i is a new variable altogether(new copy of i). Everytime setTimeout is run, the inside function forms closure with new variable i
    - But what if interviewer ask us to implement using **var**?
        
        ```jsx
        function x() {
            for(var i = 1; i<=5; i++){
            function close(i) {
                setTimeout(function() {
                console.log(i);
                }, i*1000);
                // put the setT function inside new function close()
            }
            close(i); // everytime you call close(i) it creates new copy of i. Only this time, it is with var itself!
            }
            console.log("Namaste Javascript");
        }
        x();
        ```
        
    
    ## FAMOUS INTERVIEW QUESTIONs - CLOSURES
    
    #### **Q1: What is Closure in Javascript?**
    
    **Ans**: A function along with reference to its outer environment together forms a closure. Or in other words, A Closure is a combination of a function and its lexical scope bundled together. eg:
    
    ```jsx
    function outer() {
        var a = 10;
        function inner() {
            console.log(a);
        } // inner forms a closure with outer
        return inner;
    }
    outer()(); // 10 // over here first `()` will return inner function and then using secong `()` to call inner function
    ```
    
    #### **Q2: Will the below code still forms a closure?**
    
    ```jsx
    function outer() {
        function inner() {
            console.log(a);
        }
        var a = 10;
        return inner;
    }
    outer()(); // 10
    ```
    
    **Ans**: Yes, because inner function forms a closure with its outer environment so sequence doesn't matter.
    
    #### **Q3: Changing var to let, will it make any difference?**
    
    ```jsx
    function outer() {
        let a = 10;
        function inner() {
            console.log(a);
        }
        return inner;
    }
    outer()(); // 10
    ```
    
    **Ans**: It will still behave the same way.
    
    #### **Q4: Will inner function have the access to outer function argument?**
    
    ```jsx
    function outer(str) {
        let a = 10;
        function inner() {
            console.log(a, str);
        }
        return inner;
    }
    outer("Hello There")(); // 10 "Hello There"
    ```
    
    **Ans**: Inner function will now form closure and will have access to both a and b.
    
    #### **Q5: In below code, will inner form closure with outest?**
    
    ```jsx
    function outest() {
        var c = 20;
        function outer(str) {
            let a = 10;
            function inner() {
                console.log(a, c, str);
            }
            return inner;
        }
        return outer;
    }
    outest()("Hello There")(); // 10 20 "Hello There"
    ```
    
    **Ans**: Yes, inner will have access to all its outer environment.
    
    #### **Q6: Output of below code and explanation?**
    
    ```jsx
    function outest() {
        var c = 20;
        function outer(str) {
            let a = 10;
            function inner() {
                console.log(a, c, str);
            }
            return inner;
        }
        return outer;
    }
    let a = 100;
    outest()("Hello There")(); // 10 20 "Hello There"
    ```
    
    **Ans**: Still the same output, the inner function will have reference to inner a, so conflicting name won't matter here. If it wouldn't have find a inside outer function then it would have went more outer to find a and thus have printed 100. So, it try to resolve variable in scope chain and if a wouldn't have been found it would have given reference error.
    
    #### **Q7: Advantage of Closure?**
    
    - Module Design Pattern
    - Currying
    - Memoize
    - Data hiding and encapsulation
    - setTimeouts etc.
    
    #### **Q8: Discuss more on Data hiding and encapsulation?**
    
    ```jsx
    // without closures
    var count = 0;
    function increment(){
      count++;
    }
    // in the above code, anyone can access count and change it. 
    
    ------------------------------------------------------------------
    
    // (with closures) -> put everything into a function
    function counter() {
      var count = 0;
      function increment(){
        count++;
      }
    }
    console.log(count); // this will give referenceError as count can't be accessed. So now we are able to achieve hiding of data
    
    ------------------------------------------------------------------
    
    //(increment with function using closure) true function
    function counter() {
      var count = 0;
      return function increment(){
        count++;
        console.log(count);
      }
    }
    var counter1 = counter(); //counter function has closure with count var. 
    counter1(); // increments counter
    
    var counter2 = counter();
    counter2(); // here counter2 is whole new copy of counter function and it wont impack the output of counter1
    
    *************************
    
    // Above code is not good and scalable for say, when you plan to implement decrement counter at a later stage. 
    // To address this issue, we use *constructors*
    
    // Adding decrement counter and refactoring code:
    function Counter() {
    //constructor function. Good coding would be to capitalize first letter of constructor function. 
      var count = 0;
      this.incrementCounter = function() { //anonymous function
        count++;
        console.log(count);
      }
       this.decrementCounter = function() {
        count--;
        console.log(count);
      }
    }
    
    var counter1 = new Counter();  // new keyword for constructor fun
    counter1.incrementCounter();
    counter1.incrementCounter();
    counter1.decrementCounter();
    // returns 1 2 1
    ```
    
    #### **Q9: Disadvantage of closure?**
    
    **Ans**: Overconsumption of memory when using closure as every time as those closed over variables are not garbage collected till program expires. So when creating many closures, more memory is accumulated and this can create memory leaks if not handled.
    
    **Garbage collector** : Program in JS engine or browser that frees up unused memory. In highlevel languages like C++ or JAVA, garbage collection is left to the programmer, but in JS engine its done implicitly.
    
    ```jsx
    function a() {
      var x = 0;
      return function b() {
        console.log(x);
      }
    }
     
    var y = a(); // y is a copy of b()
    y(); 
     
     // Once a() is called, its element x should be garbage collected ideally. But fun b has closure over var x. So mem of x can
    ```

<br>
<br>
<br>


## 12. FIRST CLASS FUNCTIONS AND ANONYMOUS FUNCTIONS

**What is Function statement?**

Below way of creating function are function statement.

```jsx
function a() {
  console.log("Hello");
}
a(); // Hello
```

**What is Function Expression?**

Assigning a function to a variable. Function acts like a value.

```jsx
var b = function() {
  console.log("Hello");
}
b();
```

**Difference between function statement and expression**

The major difference between these two lies in **Hoisting**.

```jsx
a(); // "Hello A"
b(); // TypeError
function a() {
  console.log("Hello A");
}
var b = function() {
  console.log("Hello B");
}
// Why? During mem creation phase a is created in memory and function assigned to a. But b is created like a variable (b:undefined) and until code reaches the function()  part, it is still undefined. So it cannot be called.
```

**What is Function Declaration? -** Other name for **function statement**.

**What is Anonymous Function? -** A function without a name.

```jsx
function () {

}// this is going to throw Syntax Error - Function Statement requires function name.
```

- They don't have their own identity. So an anonymous function without code inside it results in an error.
- Anonymous functions are used when functions are used as values eg. the code sample for **function expression** above.

**What is Named Function Expression?**

Same as Function Expression but function has a name instead of being anonymous.

```jsx
var b = function xyz() {
  console.log("b called");
}
b(); // "b called"
xyz(); // Throws ReferenceError:xyz is not defined.
// xyz function is not created in global scope. So it can't be called.
```

**Parameters vs Arguments**

```jsx
var b = function(param1, param2) { // labels/identifiers are parameters
  console.log("b called");
}
b(arg1, arg2); // arguments - values passed inside function call
```

**What is First Class Function aka First Class Citizens?**

We can pass functions inside a function as arguments and /or return a function(HOF). These ability are altogether known as First class function. It is programming concept available in some other languages too.

```jsx
var b = function(param1) {
  console.log(param1); // prints " f() {} "
}
b(function(){});

// Other way of doing the same thing:
var b = function(param1) {
  console.log(param1);
}
function xyz(){
}
b(xyz); // same thing as prev code

// we can return a function from a function:
var b = function(param1) {
  return function() {
  }  
}
console.log(b()); //we log the entire fun within b.
```

<br>
<br>
<br>


## 13. CALLBACK AND EVENT LISTENERS

### **Callback Functions**

- Functions are first class citizens ie. take a function A and pass it to another function B. Here, A is a callback function. So basically I am giving access to function B to call function A. This callback function gives us the access to whole **Asynchronous** world in **Synchronous** world.

```jsx
setTimeout(function () {
    console.log("Timer");
}, 1000) // first argument is callback function and second is timer.
```

- JS is a synchronous and single threaded language. But due to callbacks, we can do async things in JS.

```jsx
setTimeout(function () {
console.log("timer");
}, 5000);

function x(y) {
console.log("x");
y();
}

x(function y() {
console.log("y");
});
// x y timer
```

- In the call stack, first x and y are present. After code execution, they go away and stack is empty. Then after 5 seconds (from beginning) anonymous suddenly appear up in stack ie. setTimeout
- All 3 functions are executed through call stack. If any operation blocks the call stack, its called blocking the main thread.
- Say if x() takes 30 sec to run, then JS has to wait for it to finish as it has only 1 call stack/1 main thread. Never block main thread.
- Always use **async** for functions that take time eg. setTimeout, Network calls

```jsx
// Another Example of callback
function printStr(str, cb) {
    setTimeout(() => {
        console.log(str);
        cb();
    }, Math.floor(Math.random() * 100) + 1)
}
function printAll() {
    printStr("A", () => {
        printStr("B", () => {
            printStr("C", () => {})
        })
    })
}
printAll() // A B C // in order
```

### **Event Listener**

- We will create a button in html and attach event to it.

```jsx
// index.html
<button id="clickMe">Click Me!</button>

// in index.js
document.getElementById("clickMe").addEventListener("click", function xyz(){ //when event click occurs, this callback function (xyz) is called into callstack
    console.log("Button clicked");
});
```

- Lets implement a increment counter button.
    - Using global variable (not good - as anyone can change it)
        
        ```jsx
        let count = 0;
        document.getElementById("clickMe").addEventListener("click", function xyz(){ 
            console.log("Button clicked", ++count);
        });
        ```
        
    - Use closures for data abstraction
        
        ```jsx
        function attachEventList() {  //creating new function for closure
            let count = 0;
            document.getElementById("clickMe").addEventListener("click", function xyz(){
            console.log("Button clicked", ++count);  //now callback function forms closure with outer scope(count)
        });
        }
        attachEventList();
        ```
        
    
    **Garbage Collection and removeEventListeners**
    
    - Event listeners are heavy as they form closures. So even when call stack is empty, EventListener won't free up memory allocated to *count* as it doesn't know when it may need *count* again.
    - **So we remove event listeners when we don't need them (garbage collected)**
    - onClick, onHover, onScroll all in a page can slow it down heavily.

<br>
<br>
<br>


## 14. Asynchronous JavaScript & EVENT LOOP 

Browser has JS Engine which has Call Stack which has Global execution context, local execution context etc.

- But browser has many other superpowers (Web APIs) - Local storage space, Timer, place to enter URL, Bluetooth access, Geolocation access and so on.
- Now JS needs some way to connect the callstack with all these superpowers. This is done using Web APIs.

**WebAPIs**

None of the below are part of Javascript! These are extra superpowers that browser has. Browser gives access to JS callstack to use these WebAPIs

- setTimeout(), DOM APIs, fetch(), localstorage, console (yes, even console.log is not JS!!), location and so many more.
    - setTimeout() : Timer function
    - DOM APIs : eg.Document.xxxx ; Used to access HTML DOM tree. (Document Object Manipulation)
    - fetch() : Used to make connection with external servers eg. Netflix servers etc.
- We get all these inside call stack through global object ie. window
    - Use window keyword like : window.setTimeout(), window.localstorage, window.console.log() to log something inside console.
    - As window is global obj, and all the above functions are present in global object, we don't explicity write window but it is implied.

```jsx
// First a GEC is created and put inside call stack.
console.log("Start"); // this calls the console web api (through window) which in turn actually modifies values in console.

setTimeout(function cb() {  //this calls the setTimeOut web api which gives access to timer feature. It stores the callback cb() in webapi environment and starts timer.
  console.log("Callback");
 }, 5000);

console.log("End"); // calls console api and logs in console window. After this GEC pops from call stack.

/* output:
Start
End
Callback */
```

- First a GEC is created and put inside call stack.
- console.log("Start"); // this calls the console web api (through window) which in turn actually modifies values in console.
- setTimeout(function cb() { //this calls the setTimeout web api which gives access to timer feature. It stores the callback cb() and starts timer. console.log("Callback");}, 5000);
- console.log("End"); // calls console api and logs in console window. After this GEC pops from call stack.
- While all this is happening, the timer is constantly ticking. After it becomes 0, the callback cb() has to run.
- Now we need this cb to go into call stack. Only then will it be executed. For this we need **event loop** and **Callback queue**

****Event Loops and Callback Queue****

How after 5 secs timer is printed in console?

- cb() cannot simply directly go to callstack to be execeuted. It goes through the callback queue when timer expires.
- Event loop keep checking the callback queue, and see if it has any element to puts it into call stack. It is like a gate keeper.
- Once call stack is empty, eventloop pushes cb() to callstack to run. Console API is used and log printed

Another example to understand Eventloop & Callback Queue.

```jsx
console.log("Start"); 
document. getElementById("btn").addEventListener("click", function cb() { 
  // cb() registered inside webapi environment and event(click) attached to it. i.e. REGISTERING CALLBACK AND ATTACHING EVENT TO IT. 
  console.log("Callback");
});
console.log("End"); // calls console api and logs in console window. After this GEC get removed from call stack.
// In above code, even after console prints "Start" and "End" and pops GEC out, the eventListener stays in webapi env(with hope that user may click it some day) until explicitly removed, or the browser is closed.
```

In above code, even after console prints "Start" and "End" and pops GEC out, **the eventListener stays in webapi env**(with hope that user may click it some day) until explicitly removed, or the browser is closed.****

Event loop has just one job to keep checking callback queue is empty and if found something in callback queue, event loop pushes it to call stack and delete from callback queue.

**Need of callback queue?**

Suppose user clicks button x6 times. So 6 cb() are put inside callback queue. Event loop sees if call stack is empty/has space and whether callback queue is not empty(6 elements here). Elements of callback queue popped off, put in callstack, executed and then popped off from call stack.

**Behavior of fetch (Microtask Queue?)**

Let's observe the code below and try to understand

```jsx
console.log("Start"); // this calls the console web api (through window) which in turn actually modifies values in console. 
setTimeout(function cbT() { 
  console.log("CB Timeout");
}, 5000);
fetch("https://api.netflix.com").then(function cbF() {
    console.log("CB Netflix");
}); // take 2 seconds to bring response
// millions lines of code
console.log("End"); 

/*Code Explanation:
* Same steps for everything before fetch() in above code.
* fetch registers cbF into webapi environment along with existing cbT.
* cbT is waiting for 5000ms to end so that it can be put inside callback queue. cbF is waiting for data to be returned from Netflix servers gonna take 2 seconds.
* After this millions of lines of code is running, by the time millions line of code will execute, 5 seconds has finished and now the timer has expired and response from Netflix server is ready.
* Data back from cbF ready to be executed gets stored into something called a Microtask Queue.
* Also after expiration of timer, cbT is ready to execute in Callback Queue.
* Microtask Queue is exactly same as Callback Queue, but it has higher priority. Functions in Microtask Queue are executed earlier than Callback Queue.
* In console, first Start and End are printed in console. First cbF goes in callstack and "CB Netflix" is printed. cbF popped from callstack. Next cbT is removed from callback Queue, put in Call Stack, "CB Timeout" is printed, and cbT removed from callstack.
* See below Image for more understanding*/
```

![https://github.com/alok722/namaste-javascript-notes/raw/master/assets/microtask.gif](https://github.com/alok722/namaste-javascript-notes/raw/master/assets/microtask.gif)

Callback Queues are also called as MacroTask Queue or Task Queue

### **What enters the Microtask Queue ?**

- All the callback functions that come through promises go in microtask Queue.
- **Mutation Observer** : Keeps on checking whether there is mutation in DOM tree or not, and if there, then it executes some callback function.
- Callback functions that come through promises and mutation observer go inside **Microtask Queue**.
- All the rest goes inside **Callback Queue aka. Task Queue**.
- If the task in microtask Queue keeps creating new tasks in the queue, element in callback queue never gets chance to be run. This is called **starvation**

### **Some Important Questions**

1. **When does the event loop actually start ? -** Event loop, as the name suggests, is a single-thread, loop that is *almost infinite*. It's always running and doing its job.
2. **Are only asynchronous web api callbacks are registered in web api environment? -** YES, the synchronous callback functions like what we pass inside map, filter and reduce aren't registered in the Web API environment. It's just those async callback functions which go through all this.
3. **Does the web API environment stores only the callback function and pushes the same callback to queue/microtask queue? -** Yes, the callback functions are stored, and a reference is scheduled in the queues. Moreover, in the case of event listeners(for example click handlers), the original callbacks stay in the web API environment forever, that's why it's adviced to explicitly remove the listeners when not in use so that the garbage collector does its job.
4. **How does it matter if we delay for setTimeout would be 0ms. Then callback will move to queue without any wait ? -** No, there are trust issues with setTimeout() 😅. The callback function needs to wait until the Call Stack is empty. So the 0 ms callback might have to wait for 100ms also if the stack is busy.

**Working of Eventloop, Callback Queue & Microtask Queue**

![https://github.com/alok722/namaste-javascript-notes/raw/master/assets/microtask1.gif](https://github.com/alok722/namaste-javascript-notes/raw/master/assets/microtask1.gif)

![https://github.com/alok722/namaste-javascript-notes/raw/master/assets/microtask2.gif](https://github.com/alok722/namaste-javascript-notes/raw/master/assets/microtask2.gif)

![https://github.com/alok722/namaste-javascript-notes/raw/master/assets/microtask3.gif](https://github.com/alok722/namaste-javascript-notes/raw/master/assets/microtask3.gif)

![https://github.com/alok722/namaste-javascript-notes/raw/master/assets/microtask4.gif](https://github.com/alok722/namaste-javascript-notes/raw/master/assets/microtask4.gif)

![https://github.com/alok722/namaste-javascript-notes/raw/master/assets/microtask5.gif](https://github.com/alok722/namaste-javascript-notes/raw/master/assets/microtask5.gif)

![https://github.com/alok722/namaste-javascript-notes/raw/master/assets/microtask6.gif](https://github.com/alok722/namaste-javascript-notes/raw/master/assets/microtask6.gif)

<br>

## ASYNCHRONOUS JAVASCRIPT

JavaScript being single threaded means we can only do one thing at a time and the thread of execution moves down from one line to the other.

```jsx
//A reminder of how JavaScript executes code
const num = 3;
function multiplyBy2 (inputNumber){
const result = inputNumber*2;
return result;
}
const output = multiplyBy2(num);
const newOutput = multiplyBy2(10);
```

This happens line by line - synchronous javascript. Below diagram explains normal flow of javascript

![https://github.com/barry-hall/js-hard-parts-v2/raw/master/img/04-synchronous.png](https://github.com/barry-hall/js-hard-parts-v2/raw/master/img/04-synchronous.png)

### **Asynchronicity in JavaScript**

Asynchronicity is the backbone of modern web development in JavaScript yet... 

JavaScript is:

- Single threaded (one command runs at a time)
- Synchronously executed (each line is run in order the code appears)

So what if we have a task:

- Accessing Twitter's server to get new tweets that takes a long time
- Code we want to run using those tweets

**Challenge:** We want to wait for the tweets to be stored in tweets so that they're there to run displayTweets on - but no code can run in the meantime.

```jsx
//Slow function blocks further code running
const tweets = getTweets("http://twitter.com/will/1")
// ⛔350ms wait while a request is sent to Twitter HQ
displayTweets(tweets)
// more code to run
console.log("I want to runnnn!")
```

JavaScript is not enough—We need a new pieces (some of which aren't JavaScript at all)

Our core JS engine has 3 main parts:

- Thread of execution
- Memory/variable environment
- Call stack

We need to add some new components:

- Web Browser APIs/Node background APIs
- Promises
- Event loop, Callback/Task queue and micro task queue

### **Asynchronous Browser Features**

JavaScript often runs in the browser. The web browser has a whole bunch of extra functionaltiy beyond JS, to name a few:

- Dev tools
- Web sockets
- Network functionality
- Rendering engine
- HTML Dom

Although we don't find these features in JS, we can interface with them.

Some examples of these interfaces are:

- setTimeout -> Timer
- document -> HTMLDom
- xhr/fetch -> Network Requests
- console -> console

There are other things like "local storage", "indexdb" etc. What's important is to realise that these things aren't features of JavaScript itself.

### **Web API - Async Code Example**

ES5 solution: Introducing ‘callback functions’, and Web Browser APIs

```jsx
function printHello() { console.log("Hello");}
setTimeout(printHello, 1000);
console.log("Me first!");

/* output: 
Me first
Hello 
*/
```

This is processed / broken down as below:
![image](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/c389e139-c761-432e-892c-7cb64d63d090/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220526%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220526T175257Z&X-Amz-Expires=86400&X-Amz-Signature=950f3e7b636a28bb8a51ef1c5df840b9c91d72cb35d681dd82da6a5af0e94c08&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

### **Callback Queue & Event Loop**

When we use things like `setTimeout` there is another queue in play. This is called the event loop. It is a queue of callbacks.

All code on the call stack has to be completed before anything from the event loop will be put on that stack. If the call stack is empty, it checks the queue and puts anything in there on the call stack. If not it just waits.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2a96129b-6d46-4baf-b9af-4c498c90ec42/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220526%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220526T175430Z&X-Amz-Expires=86400&X-Amz-Signature=b88f275b8c629cf4911703a03f4181026d3fb472b7c5999911c5a7e75d56679d&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

**ES5 Web Browser APIs with callback functions**

**Problems**

- Our response data is only available in the callback function - Callback hell
- Maybe it feels a little odd to think of passing a function into another function only for it
to run much later

**Benefits**

- Super explicit once you understand how it works under-the-hood
<br>
<br>
<br>

## 15.Promises - **Introduction**

Promises are a two-pronged 'facade' funtions that both:

- Initiate background web browser work and
- Return a placeholder object (promise) immediately in JavaScript
- Special objects built into JavaScript that get returned immediately when we make
a call to a web browser API/feature (e.g. fetch) that’s set up to return promises
(not all are)
- Promises act as a placeholder for the data we expect to get back from the web
browser feature’s background work

**Promise Example - Fetch and .then**

```jsx
function display(data) {
    console.log(data);
}

const futureData = fetch('http://twitter.com/will/tweets/1');
futureData.then(display);

console.log("Me first!");
```

Fetch is a browser feature that is able to talk to the outside world. It also returns a little 'bonus' object that we are able to use in JS.

**How fetch works behind the scenes.**

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/5ff7dbd1-f19e-4c94-b73e-3d4ee8945b88/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220526%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220526T175759Z&X-Amz-Expires=86400&X-Amz-Signature=19c2fbab4153acadbfc74b1f938715908fe08dede969a02ad904451f81a3d2d9&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- Fetch initiates the network request through the web browser, when the request is completed the value is placed within the "futureData" variables "value" property.****
- When the promise is complete and the data has been resolved and assigned within the promise object, then you can access it via the `then` method.
- In the example, the display function is passed into the array of futureData's onFulfilled property to be run when completed.
- Once the data comes back from twitter, the value is placed in `futureData`'s value property. This in turn triggers the onFulfilled array of functions to run. In this case `display`.
- ***then* method and functionality to call on completion...**
    
    Any code we want to run on the returned data must also be saved on the promise object. Added using `.then` method to the hidden property `onFulfilment`. Promise objects will automatically trigger the attached function to run (with its input being the returned data).
    

But we need to know how our promise-deferred functionality gets back into JavaScript to be run

**MICROTASK QUEUE**

**Another Example for Web APIs & Promises - fetch and .then** 

```jsx
function display(data){console.log(data)}
function printHello(){console.log("Hello");}
function blockFor300ms(){/* blocks js thread for 300ms */ }
setTimeout(printHello, 0);
const futureData = fetch('[https://twitter.com/will/tweets/1](https://twitter.com/will/tweets/1)')
futureData.then(display)
blockFor300ms()
console.log("Me first!");

/*output:
Me first (after 300ms)
Hi
Hello
*/

```

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2b49b859-3bf9-4f2b-ac79-b78bbd8da31d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220526%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220526T175824Z&X-Amz-Expires=86400&X-Amz-Signature=3b35dc3418cfc065a8bd99bf7cbc6c60028093288f228e4ab86bc699a85c8ff2&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- The promise object is designed by JS to automatically trigger the functions in the array of `onCompleted` to run with the value of the promise as the input, when the value gets updated (e.g. when resolved the tweets from a twitter api call).
- When you add a function with `then` this function gets added to the `onCompletion` array. When the value gets updated (e.g. when a network call resolves) the then function is called with the value field passed as the argument.
- The microtask queue is used when you create promises. This is invoked as part of the promise chain. Any function that is attached to a promise object via the `then` method doesn't go into the callback queue, it goes into the microtask queue. When there is nothing on the callstack the event loop will first check the microtask queue then the callback queue.

### **Promises Review**

Problems:

- 99% of developers have no idea how (Promises) are working under the hood
- Debugging becomes super-hard as a result
- Developers fail technical interviews

Benefits:

- Cleaner readable style with pseudo-synchronous style code
- Nice error handling process

If you want to add a function to the `onRejection` array of a promise object (the array that runs when things fail) then you can do this in a couple of ways.

1. `futureData.catch(someFunction)`
2. `futureData.then(successFunction, rejectFunction)`

**We have rules for the execution of our asynchronously delayed code**. 

Hold promise-deferred functions in a microtask queue and callback function in a task queue (Callback queue) when the Web Browser Feature (API) finishes.

Add the function to the Call stack (i.e. run the function) when:

- Call stack is empty & all the global code run (Have the Event Loop check this condition)

Prioritize functions in the microtask queue over the Callback queue.

**Promises, Web APIs, the Callback & Microtask Queues and Event loop enable:**

- **Non-blocking applications**: This means we don’t have to wait in the single thread
and don’t block further code from running
- **However long it takes**: We cannot predict when our Browser feature’s work will
finish so we let JS handle automatically running the function on its completion
- **Web applications**: Asynchronous JavaScript is the backbone of the modern web -
letting us build fast ‘non-blocking’ applications
<br>
<br>

## 16. JS Engine, Google's V8 Architecture

- JS runs literally everywhere from smart watch to robots to browsers because of Javascript Runtime Environment (JRE).
- JRE is like a big container which has everything which are required to run Javascript code.
- JRE consists of a JS Engine (❤️ of JRE), set of APIs to connect with outside environment, event loop, Callback queue, Microtask queue etc.
- Browser can execute javascript code because it has the Javascript Runtime Environment.
- ECMAScript is a governing body of JS. It has set of rules which are followed by all JS engines like Chakra(Edge), Spidermonkey(Firefox)(first javascript engine created by JS creator himself), v8(Chrome)
- Javascript Engine is not a machine. Its software written in low level languages (eg. C++) that takes in hi-level code in JS and spits out low level machine code.
- Code inside Javascript Engine passes through 3 steps : **Parsing**, **Compilation** and **Execution**
    1. **Parsing** - Code is broken down into tokens. In "let a = 7" -> let, a, =, 7 are all tokens. Also we have a syntax parser that takes code and converts it into an AST (Abstract Syntax Tree) which is a JSON with all key values like type, start, end, body etc (looks like package.json but for a line of code in JS. Kinda unimportant)(Check out astexplorer.net -> converts line of code into AST).
    2. **Compilation** - JS has something called Just-in-time(JIT) Compilation - uses both interpreter & compiler. Also compilation and execution both go hand in hand. The AST from previous step goes to interpreter which converts hi-level code to byte code and moves to execeution. While interpreting, compiler also works hand in hand to compile and form optimized code during runtime. **Does JavaScript really Compiles?** The answer is a loud **YES**. More info at: [Link 1](https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/get-started/ch1.md#whats-in-an-interpretation), [Link 2](https://web.stanford.edu/class/cs98si/slides/overview.html), [Link 3](https://blog.greenroots.info/javascript-interpreted-or-compiled-the-debate-is-over-ckb092cv302mtl6s17t14hq1j). JS used to be only interpreter in old times, but now has both to compile and interpreter code and this make JS a JIT compiled language, its like best of both world.
    3. **Execution** - Needs 2 components ie. Memory heap(place where all memory is stored) and Call Stack(same call stack from prev episodes). There is also a garbage collector. It uses an algo called **Mark and Sweep**.
