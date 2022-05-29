
## Classes & Prototypes - Object Oriented JavaScript

- An enormously popular paradigm for structuring our complex code
- Prototype chain - the feature behind-the-scenes that enables emulation of OOP but is a compelling tool in itself
- Understanding the difference between `__proto__` and `prototype`
- The `new` and `class` keywords as tools to automate our object & method creation

### **Core of development (and running code)**

1. Save data (e.g. in a quiz game the scores of user1 and user2)
2. Run code (functions) using that data (e.g. increase user 2's score)

Easy! So why is development hard?

In a quiz game I need to save lots of users, but also admins, quiz questions, quiz outcomes, league tables - all have data and associated functionality

In 100,000 lines of code

- Where is the functionality when I need it?
- How do I make sure the functionality is only used on the right data!

### **That is, I want my code to be**

1. Easy to reason about, But also
2. Easy to add new features to (new functionality)
3. Nevertheless efficient and performant

The Object-oriented paradigm aims is to let us achieve these three goals.

## ****Object Dot Notation****

So if I'm storing each user in my app with their respective data (let's simplify)

User 1 - name: 'Tim', score: 3
User 2 - name: 'Stephanie', score: 5

And the functionality I need to have for each user (again simplifying)

- increment functionality (in practice, there will be many functions)

How could I store my data and functionality together in one place?

### **Objects store functions with their associated data**

This is the principle of encapsualtion - and it's going to transform how we can 'reason about' our code!

```jsx
const user1 = {
    name: "Sid",
    score: 3,
    increment: function() {user1.score ++}
};

user1.increment(); //user1.score -> 4
```

***Let's keep creating our objects. What alternative techniques do we have for creating these objects?***

### **Creating user2 using dot notation**

Declare an empty object and add properties with dot notation

```jsx
const user2 = {};

//assign properties to that object
user2.name =  "Lem";
user2.score = 6;
user2.increment = function() {
    user2.score++;
};
```

### Creating user3 using Object.create

Object.create is going to give us fine-grained control over our object later on

```jsx
const user3 = Object.create(null);
[user3.name](http://user3.name/) = "Eva";
user3.score = 9;
user3.increment = function() {
user3.score++;
};
```

Our code is getting repetitive, we're breaking our DRY principle. And suppose we have millions of users! What could we do?

### Solution 1. Generate objects using a function

***Factory Functions Example:***

```jsx
function userCreator(name, score) {
    const newUser = {};
    newUser.score = score;
    newUser.increment = function() {
        newUser.score++;
    };
    return newUser;
};

const user1 = userCreator("Will", 3);
const user2 = userCreator("Tim", 5);
user1.increment()
```

This is what's known as a factory function. This is a function that returns an object. It’s like a factory, we give the arguments, it creates object and return us that object.****

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0ce7a421-f454-4f10-b2bb-7bef4bc02e9e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220529%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220529T180314Z&X-Amz-Expires=86400&X-Amz-Signature=76cf3eec1bb580ba4fd57a4310f492bc88e712180b36cc425063ca6b8aab5c86&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

***Solution 1 Summary***. Generate objects using a function

***Problems***: Each time we create a new user we make space in our computer's memory for all our data and functions. But our functions are just copies. Is there a better way?

***Benefits***: It's simple and easy to reason about!

### Solution 2: Using the prototype chain

Rather than having each user object contain an increment function, ideally you want *one* increment function that is the same but contained on each of the user objects.

Store the increment function in just one object and have the interpreter, if it doesn't find the function on user1, look up to that object to check if it's there.

Link user1 and fcuntionStore so the interpreter, on not finding `.increment` makes sure to check up in the functionStore where it would find it.

Make the link with `Object.create()` technique.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/d48b600b-073f-4e8a-9a62-183ba9d2bbda/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220529%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220529T180819Z&X-Amz-Expires=86400&X-Amz-Signature=386614a67370c973360d20649ab993cac180c567f8136922ec5b877a537e0458&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

```jsx
function userCreator(name, score) {
    const newUser = Object.create(userFunctionStore);
    newUser.name = name;
    newUser.score = score;
    return newUser;
};

const userFunctionStore = {
    increment: function(){this.score++},
    login: function(){console.log("Logged in");}
};

const user1 = userCreator("Will", 3);
const user2 = userCreator("Tim", 5);

user1.increment();
```

### ***Prototype Chain Example: Prototypal Link***

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/1b71fad0-eaa4-4d11-9bfd-ce96e633532f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220529%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220529T180854Z&X-Amz-Expires=86400&X-Amz-Signature=b9238ea2b234c5f6210148db6dabf277cc83aff7ed09ea4f4ec92b0ea4e1e96c&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

When JS doesn't find a given property (method or data) on an object it goes to the proto property and looks up the prototype chain to see if it can find that method or data.****

****Prototype Chain Example: Implicit Parameters****

When you use `Object.create()` it creates an empty object with hidden `__proto__` property.

When you call `.increment()` on an object JS will look up the prototype chain to see if it can find the method. It does, and on the left of the .increment call `this` will be bound to the object the call has been invoked on. e.g. `user1.increment()`. This will access user1's score property that is used to increment. `this` refers to what is on left side of dot.

****Prototype Chain Example: hasOwnProperty Method**** 

What if we want to confirm our user1 has the property score?

```jsx
function userCreator (name, score) {
 const newUser = Object.create(userFunctionStore);
 newUser.name = name;
 newUser.score = score;
 return newUser;
};
const userFunctionStore = {
 increment: function(){this.score++;},
 login: function(){console.log("Logged in");}
};
const user1 = userCreator("Will", 3);
const user2 = userCreator("Tim", 5);
**user1.hasOwnProperty('score')**
```

**We can use the hasOwnProperty method - but where is it? Is it on user1?**
All objects have a **proto** property by default which defaults to linking to a big object Object.prototype full of (somewhat) useful functions. We get access to it via userFunctionStore’s **proto** property - the chain

When you run the `user1.hasOwnProperty('score')` what happens is JS looks for the method "hasOwnProperty" on the user1 object. It doesn't exist so it looks up the chain to userFunctionStore. This doesn't have it either so it follows it's proto chain until it hits `Object.prototype` which *does* have the hasOwnProperty method. At which point it's grabbed and ran.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a0c4afcb-c6b1-426d-8a83-871f7f98ae3a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220529%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220529T180925Z&X-Amz-Expires=86400&X-Amz-Signature=0c0d60aed5ad82e15cc8d2c7ef9f135c1594393dce4fb7d973b72b92b088eb68&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

### This Keyword

Now let’s change our code little bit.

Declaring & calling a new function inside our ‘method’ increment

```jsx
function userCreator(name, score) {
	const newUser = Object.create(userFunctionStore);
	[newUser.name](http://newuser.name/) = name;
	newUser.score = score;
	return newUser;
};

const userFunctionStore = {
 increment: function() {
 function add1(){ this.score++; }
 add1()
 }
};

const user1 = userCreator("Will", 3);
const user2 = userCreator("Tim", 5);
user1.increment();
```

Now, What does this refers to inside **increment function,** 

What does this get auto-assigned to?

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/12fbba55-33b6-4ccf-8458-762a6b1c5b21/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220529%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220529T181007Z&X-Amz-Expires=86400&X-Amz-Signature=bc497a7c76c8d116dfa001fff8d1e15dfdc57ca0b5443e1cca67c54772bd3739&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

When we call add1() function, this refers to global object, because on left side of add1()function there is nothing for eg: user1.add1(), So by default, this keyword in normal function refers to global window object.

We can solve this problem by using `call()` method. Instead of calling add1() directly we can use

```jsx
add1.call(this) /* here this will evaluate to user1, because we are inside 
execution context of user1 */
```

There is also another clean way of solving this problem. By using **arrow functions,**  

arrow functions don’t have their this keyword, if we use this inside arrow function, this is lexically scoped and refers to where the function was saved or stored, in our eg: add1() is saved inside UserCreator, so when we call user1.increment(), this refers to user1.

```jsx
function userCreator(name, score) {
 const newUser = Object.create(userFunctionStore);
 newUser.name = name;
 newUser.score = score;
 return newUser;
};
const userFunctionStore = {
 increment: function() {
 const add1 = () => { this.score++; }
 add1()
 }
};
const user1 = userCreator("Will", 3);
const user2 = userCreator("Tim", 5);
user1.increment();
```

**Now our inner function gets its this set by where it was saved - it’s a ‘lexically scoped this**

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/bbfc0255-f563-40df-8ae6-8fdbc568dec0/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220529%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220529T181032Z&X-Amz-Expires=86400&X-Amz-Signature=4e8d50cb3fbca24ff21cf88e913dba755c59198fcbc4da1a5451abb4c871d23b&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

***Solution 2***: Using the prototype chain

***Problems:*** No problems! It's beautiful. Maybe a little long-winded Write this every single time 

***Benefits***: Super sophisticated(explicit) but not standard

### Solution 3 - Introducing the keyword - new

Introducing the keyword that automates the hard work: new

When we call the function that returns an object with new in front we automate 2
things

1. Create a new user object
2. Return the new user object
But now we need to adjust how we write the body of userCreator - how can we:
    - Refer to the auto-created object?
    - Know where to put our single copies of functions?

### **Interlude Explanation** : (**Functions are both objects and functions**)

Functions are both objects and functions

```jsx
function multiplyBy2(num){
 return num*2
}
multiplyBy2.stored = 5
multiplyBy2(3) // 6
multiplyBy2.stored // 5
multiplyBy2.prototype // {}
```

We could use the fact that all functions have a default property `prototype` on their object version, (itself an object) - to replace our `functionStore` object

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3421af9c-52ee-4739-a6d0-8d7788222e5a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220529%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220529T181324Z&X-Amz-Expires=86400&X-Amz-Signature=0745e65a2f605bde40345c9674c8f19b18c089f1a7895f1e262ceaa2313f876d&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

```jsx
//The new keyword automates a lot of our manual work
function userCreator(name, score) {
 ~~const newUser = Object.create(functionStore);~~
 ~~newUser~~ this.name = name;
 ~~newUser~~ this.score = score;
 ~~return newUser;~~
};

~~functionStore~~ userCreator.prototype // {};
~~functionStore~~ userCreator.prototype.increment = function(){
 this.score++;
}

const user1 = new userCreator("Will", 3);
```

Working of Solution3 is exactly same as solution2, But new keywords does the work behind the scenes

The new keyword automates a lot of our manual work

```jsx
function userCreator(name ,score) {
    this.name = name;
    this.score = score;
}

userCreator.prototype.increment = function() { this.score ++ };
userCreator.prototype.login = function() { console.log("login"); };

const user1 = new userCreator("Eva", 9);

user1.increment();
```

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b25a3229-c32c-482c-b1a2-5fe36df662ef/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220529%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220529T181403Z&X-Amz-Expires=86400&X-Amz-Signature=4feffd004608f7159ebd40613e568515f4709d96a3d2523f18527196bdd629e3&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

When we call a function with new keyword, it automatically creates an empty object with `this`

and it’s __proto__ is linked to userCreator, finally the object is returned from the function.

***Solution 3 -*** 

***Benefits:*** Faster to write. Often used in practice in professional code <br>
***Problems:*** 95% of developers have no idea how it works and therefore fail interviews
We have to upper case first letter of the function so we know it requires ‘new’ to
work!

## Solution 4: The ES6 class ‘syntactic sugar’

We’re writing our shared methods separately from our object ‘constructor’ itself (off in the userCreator.prototype object)
Other languages let us do this all in one place. ES2015 lets us do so too

```jsx
class UserCreator {
 constructor (name, score){
	 this.name = name;
	 this.score = score;
 }
 increment (){ this.score++; }
 login (){ console.log("login"); }
}
const user1 = new UserCreator("Eva", 9);
user1.increment();
```

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/34168cd6-0df0-46ec-ac1f-b36a41bf9165/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220529%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220529T181547Z&X-Amz-Expires=86400&X-Amz-Signature=12b5beeed56c7041291e022ad154d69e7b2e69cb2a3097ab881e12e15b4c3059&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

The above works exactly as solution 3 under the hood, class is just syntactic sugar over functions.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/55d79a1d-6a59-4ea0-b5a2-ac5aa911d364/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220529%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220529T181552Z&X-Amz-Expires=86400&X-Amz-Signature=254df7abad119f4245cb89ddc012f56207a7aceac77e432d0b1634dde87630f1&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

***Solution4:***

***Benefits***: Emerging as a new standard and Feels more like style of other languages (e.g. Python)
***Problems***: 99% of developers have no idea how it works and therefore fail interviews
But we will not be one of them!
