# Table of Contents

- [Programming-Notes](#programming-notes)
- [Java](#java)
- [JavaScript (JS)](#javascript-js)
  - [`this` Keyword](#this-keyword)
    - [Function Context](#function-context)
  - [React](#react)
  - [Functions](#functions)
    - [Arrow Functions](#arrow-functions)
    - [Function Expressions](#function-expressions)
    - [Function Binding](#function-binding)
  - [Error Handling](#error-handling)
  - [Clean Code: Rules of Thumb](#clean-code-rules-of-thumb)
  - [DOM Manipulation and Events](#dom-manipulation-and-events)
    - [Targeting Nodes with Selectors](#targeting-nodes-with-selectors)
    - [Element Creation](#element-creation)
  - [Events](#events)
  - [Promises](#promises)
  - [Node.js](#nodejs)
  - [Objects](#objects)
    - [Prototypes](#prototypes)
    - [Factory Functions](#factory-functions)
    - [Private Variables and Closure](#private-variables-and-closure)
    - [Getters and Setters](#getters-and-setters)
- [Back-End Web Architecture](#back-end-web-architecture)
  - [Mapping Out a Request](#mapping-out-a-request)
- [S.O.L.I.D Design Principles](#solid-design-principles)
- [Object Role Stereotypes](#object-role-stereotypes)
- [Miscellaneous](#miscellaneous)
- [Networking](#networking)


# Programming-Notes
notes to reference as i learn programming and the concepts around it

# Java

```int e = 20/6;
   System.out.println("e is " + e); // e is 3.
```

In Java this is called truncation. Remainder gets dumped, and only the integer is saved into the variable.

![Implicit Conversion](https://user-images.githubusercontent.com/92711276/214709495-d2a8497a-c6eb-4427-95d2-7c9c2f0499db.PNG)

![Explicit Conversion](https://user-images.githubusercontent.com/92711276/214714303-f3c20fc8-069f-45c5-85ec-3f5f0a650bcf.PNG)

In explicit conversions, if you're assigning the result of an operation between two like types to a variable, the type of the resultant variable will be whate it was declared as. Example:

```
int a = 20;
int b = 10;
double c = a/b;
System.out.println("c is " + c); // c is 2.0
```

* Overriding equals() in classes when inheritance is involved: http://users.csc.calpoly.edu/~gfisher/classes/102/info/howToOverrideEquals.html

# JavaScript (JS)
* When comparing values of different types, JS converts the values to numbers first
* **_null_** is its own data type and intentionally sets a value to empty. **_undefined_** is also its own type and is the default initial value for declared variables that havent been assigned anything yet. generally not good practice 
* operands of different types are converted to numbers by the equality operator ==. An empty string, just like false, becomes a zero. Ex: alert( '' == false ); // true
* But alert( '' === false ); // false because '===' is strict, instead of testing for equivalence. so it reads as "is 0 the **exact same** as false"
* When doing comparisons between strings, JS goes character by character.
 
  Eg. 'glow' > 'glee' returns true. why? g == g, move on to next character in both words ('l', which are equal),
  then goes on and compares 'o' and 'e', where it shows 'o' is greater according to the Unicode indexing JS uses
* For maths and other comparisons (< > <= >=)
  null/undefined are converted to numbers: null becomes 0, while undefined becomes NaN.
* undefined == null, undefined, and no other value. 
* Precedence of AND && is higher than OR ||

So the code a && b || c && d is essentially the same as if the && expressions were in parentheses: (a && b) || (c && d).

* Boolean Operators return a value, which is usually ana operand.

  Ex: The AND && operator does the following:

   Evaluates operands from left to right.
   For each operand, converts it to a boolean. If the result is false, stops and returns the original value of that operand.
   
* The number 0, an empty string "", null, undefined, and NaN are all **falsy** values. Everything else is **truthy**.
* Getting started with DevTools: https://developer.chrome.com/docs/devtools/

## `this` keyword

The value of this depends on in which context it appears: function, class, or global.

### Function context

Inside a function, the value of this depends on **how the function is called**. Think about `this` as a hidden parameter of a function.

```
function getThis() {
  return this;
}

const obj1 = { name: "obj1" };
const obj2 = { name: "obj2" };

obj1.getThis = getThis;
obj2.getThis = getThis;

console.log(obj1.getThis()); // { name: 'obj1', getThis: [Function: getThis] }
console.log(obj2.getThis()); // { name: 'obj2', getThis: [Function: getThis] }
```

The value of this is not the object that has the function as an own property, **but the object that is used to call the function.** You can prove this by calling a method of an object up in the prototype chain.

```
const obj3 = {
  __proto__: obj1,
  name: "obj3",
};

console.log(obj3.getThis()); // { name: 'obj3' }
```

The value of this always changes based on how a function is called, even when the function was defined on an object at creation:

```
const obj4 = {
  name: "obj4",
  getThis() {
    return this;
  },
};

const obj5 = { name: "obj5" };

obj5.getThis = obj4.getThis;
console.log(obj5.getThis()); // { name: 'obj5', getThis: [Function: getThis] }
```

## React

* _props_ is an empty object that gets passed "key:value", and values are accessed via the key. ex. `props.key = value`
* Let’s quickly recap what’s going on and the order in which the methods are called:

```
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {    this.setState({      date: new Date()    });  }
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<Clock />);
``` 

1. When <Clock /> is passed to root.render(), React calls the constructor of the Clock component. Since Clock needs to display the current time, it initializes this.state with an object including the current time. We will later update this state.

2. React then calls the Clock component’s render() method. This is how React learns what should be displayed on the screen. React then updates the DOM to match the Clock’s render output.

3. When the Clock output is inserted in the DOM, React calls the componentDidMount() lifecycle method. Inside it, the Clock component asks the browser to set up a timer to call the component’s tick() method once a second.

4. Every second the browser calls the tick() method. Inside it, the Clock component schedules a UI update by calling setState() with an object containing the current time. Thanks to the setState() call, React knows the state has changed, and calls the render() method again to learn what should be on the screen. This time, this.state.date in the render() method will be different, and so the render output will include the updated time. React updates the DOM accordingly.

5. If the Clock component is ever removed from the DOM, React calls the componentWillUnmount() lifecycle method so the timer is stopped.



## Functions

* Form of an anonymous function:

  ```
  function myFunction() {
     alert('hello');
  }
  ```
 
  turns into: 
  
  ```
  (function () {
      alert('hello');
  })
  ```
  
  i.e. drops the function name, and encloses the function definition in parentheses
  
### Arrow Functions

* These are another form of anonymous functions:

  ```
  function myFunction() {
     alert('hello');    
  }
  ```
  
  which is similar to: 
  
  ```
  () => {
  alert('hello'); 
  }
  ```
  
  which is similar to
  
  `() => alert('hello');`
 
 * If the function has one parameter, then:

     ```
     function favoriteAnimal(animal) {
     console.log(animal + " is my favorite animal!")
   }
     ```
     
    is the same (anonymously) as:
    
    `(animal) => console.log(animal + " is my favorite animal!")`
    
    and we can go further by dropping the parentheses around the parameter:
    
    `animal => console.log(animal + " is my favorite animal!")`
    
* Finally, if your function needs to return a value, and contains only one line, you can also omit the return statement:

  ```
  function doubleItem(item) {
   return item * 2;
  }
  ```
 
    in arrow form:
 
   `(item) => item * 2`
   
### Function Expressions

 ```
 let sayHi = function() {
  alert( "Hello" );
};
```

The above is a _function express_ that's assigned to the variable "sayHi". 
Note the semicolon at the end, as the function expression is still part of an assignment statement

`alert( sayHi ); // shows the function code`



The key thing about JavaScript expressions is that they return values. 

```
(function() {
  console.log('hello');
})
```

In the snippet above the return value of the expression is the function itself. If we were to log the expression using `console.log()` the function definition would be printed. Therefore, to _immediately invoke_ the returned _function expression_, we could just tack `()`s on to the end and "hello" would be printed. In case it still wasn't obvious, this is what we call an **IIFE**, or **Immediately Invoked Function Expression.**

The primary use case for IIFEs is to secure data.

You can also pass arguments into IIFEs like so:

```
var foo = "foo";

(function (innerFoo) {
    // Outputs: "foo"
    console.log(innerFoo);
})(foo);
```

### Function Binding

```
x = 9;
var module = {
    x: 81,
    getX: function () {
        return this.x;
    }
};

module.getX(); // 81

var getX = module.getX;
getX(); // 9, because in this case, "this" refers to the global object

// create a new function with 'this' bound to module
var boundGetX = getX.bind(module);
boundGetX(); // 81
```

`.bind()` takes an object and returns a function that binds its context (`this`) to the passed-in object.

## Variables

![global-vars](https://github.com/user-attachments/assets/755f787f-9e03-4f87-85f0-88ecff356f60)

## Error Handling

```
const a = 5;
const b = 10;

function add() {
  return c;
}

function print() {
  add();
}

print();
```

<img width="620" alt="stack-trace" src="https://user-images.githubusercontent.com/92711276/185812484-5de6bd9b-b1fa-4eac-bfde-c0c622f5fd28.png">

* The stack trace above tells us that:

    1) c is not defined in scope of add(), which is declared on line 5
    2) add() was called by print(), which was declared on line 9
    3) print() itself was called on line 12.

## Clean Code: Rules of Thumb

* Indentation: Stay consistent. If you start with 2 space indents, keep it consistent all throughout.
* Semicolons: Always add them. Period.
* Line length: 
   - Try to break each line at around **80** characters.
   - Also try to break after an **operator** or **comma**
   - Then, there are a few ways to format continuation lines. For example:
     * indent continuation lines by one level
     * vertically align continuation lines with the first variable

* Naming things:
  - always use camelCase
  - _variables_ should be **nouns** or **adjectives**. _Functions_ should be **verbs**. Examples:
    ```
    const numberOfThings = 10;
    const myName = "Thor";
    const selected = true;
    ```
    
## DOM Manipulation and Events 

* The DOM is a tree-like representation of the elements on a page. Each element is a "node", with nodes on the same level considered "siblings" of each other.

![dom-tree](https://user-images.githubusercontent.com/92711276/186529303-1d3afba3-c13a-48b1-b0cb-225ba633e305.png)

### Targeting Nodes with Selectors

* When working with the DOM, you use “selectors” to target the nodes you want to work with. You can use a combination of CSS-style selectors and relationship properties to target the nodes you want.

`<div class="display"></div>` is a “child” of `<div id="container"></div>` and a sibling to `<div class="controls"></div>`. 

`<div id="container"></div>` is a parent, with its children on the next level, each on their own “branch”.

![html snippet](https://user-images.githubusercontent.com/92711276/186530786-4ffcf735-f723-4f0c-8c7b-b5cc7d941ce7.png)

- CSS-style selectors:

  * div.display
  * .display
  * #container > .display
  * div#container > div.display
  
![js snippet](https://user-images.githubusercontent.com/92711276/186531765-02711d0f-1f64-4032-aa56-d7f4e9205d77.png)

- Relational selectors:

  * container.firstElementChild
  * controls.previousElementSibling

### Targeting Nodes with Selectors  

HTML is converted to the DOM when parsed by the browser. The nodes of the DOM are objects, with properties and methods.

* Query Selectors
  - **element.querySelector(selector)** returns a reference to the first match of _selector_
  - **element.querySelectorAll(selectors)** returns a “nodelist” containing references to all of the matches of the _selectors_
     _note that when using **querySelectorAll**, the return value is NOT an array. It looks like an array, and it somewhat acts like an array, but it’s            really a “nodelist”. The big distinction is that several array methods are missing from nodelists._ 
     
     This nodelist can be converted to an array using Array.from() or the spread operator.
     
### Element Creation
        
`document.createElement(tagName, [options])` creates a new element of tag type `tagName`. `[options]` in this case means you can add some optional parameters to the function.

`const div = document.createElement('div');`

this function does not put your new element into the DOM. it's saved into memory so you can further make adjustments until you're ready to finally insert into the DOM with _other_ functions like:

 * **parentNode.appendChild(childNode)** - which appends _childNode_ as the last child of _parentNode_
 * **parentNode.insertBefore(newNode, referenceNode)** - which inserts _newNode_ into _parentNode_ before _referenceNode_

**Keep in mind:** JavaScript **DOES NOT** alter your HTML. It alters the DOM - your HTML file will look the same, but the Javascript changes what the browser renders.

JavaScript, for the most part, is run whenever the JS file is run, or when the script tag is encountered in the HTML. 

If included at the top of your file, many of these DOM manipulation methods will not work because the JS code is being run **before** the nodes are created in the DOM. 

The simplest way to fix this is to include your JavaScript at the bottom of your HTML file so that it gets run after the DOM nodes are parsed and created.

_Alternatively_, you can link the JavaScript file in the `<head>` of the HTML document. The `<script>` tag with the `src` attribute containing the path to the JS file can be used, alongside the `defer` keyword to load the file after the HTML is parsed, as such:

```
<head>
  <script src="js-file.js" defer></script>
</head>
```

## Events

Events in JS are exactly what they sound like: an action that happens to an element(s) on a page or in the browser environment.
Examples include "clicks", "mouseovers", "keypresses", browser reloading etc. 

Javascript listens for these actions and reacts based on the "event listeners" set on each element. There are three primary ways to go about adding event listeners:

 * **you can attach functions’ attributes directly on your HTML elements**
 
 ![image](https://user-images.githubusercontent.com/92711276/186788358-66b95d5b-9d48-42ee-bd44-f8406752291d.png)
 
 * **you can set the “on_event_” property on the DOM object in your JavaScript**
 
 ![image](https://user-images.githubusercontent.com/92711276/186788378-1613d34d-8c82-45ab-aeca-19e8bf073aa1.png)
 ![image](https://user-images.githubusercontent.com/92711276/186788322-3ed12553-da2c-4cb2-89a0-8ed316b35030.png)

 * **you can attach event listeners to the nodes in your JavaScript**
 
 ![image](https://user-images.githubusercontent.com/92711276/186788572-1290d221-afe6-4795-b76a-cce8b0c179a5.png)
 ![image](https://user-images.githubusercontent.com/92711276/186788599-98ae463c-4129-442b-8c5c-e194e6d02c1f.png)
 
## Promises

Everything is explained super clearly at https://javascript.info/promise-basics
 
## Node.js

Node.js is a JavaScript runtime environment that allows you to run JavaScript outside of your web browser. 


## Objects

An object can be created with figure brackets {…} with an optional list of properties. A **property** is a “key: value” pair, where key is a _string_ (also called a “property name”), and value can be anything.


### Prototypes

The prototype field of an object (`__proto__`) points to the object it inherits from. Or...the object that is its **prototype**.

We get the prototype of an object with the following method: `Object.getPrototypeOf(object)`
We set the prototype of an object with the following method: `Object.setPrototypeOf(object)`

![Screenshot 2022-12-22 at 15-36-23 Prototypal inheritance](https://user-images.githubusercontent.com/92711276/209225739-e85cfd4d-c0ee-43c8-a3f8-fa1659682ae9.png)

![Screenshot 2022-12-22 at 16-11-26 Prototypal inheritance](https://user-images.githubusercontent.com/92711276/209227261-e1c24327-2785-480f-bf51-180882b3a812.png)

![Screenshot 2022-12-22 at 16-11-42 Prototypal inheritance](https://user-images.githubusercontent.com/92711276/209227282-d38b3996-e346-4c12-9eb6-21ed1b2899df.png)

The `Object.create()` method creates a new object, using an existing object as the prototype of the newly created object. 

```
const person = {
  isHuman: false,
  printIntroduction: function() {
    console.log(`My name is ${this.name}. Am I human? ${this.isHuman}`);
  }
};

const me = Object.create(person); // (/)
```
(/) this line says: create a new object from `person`, dump it in `me`, then set `me.__proto__` to `person`

#### Function Protoypes

![prototypes](https://user-images.githubusercontent.com/92711276/187792892-d50f4463-bd73-4b98-ba12-58605cf2d87f.png)

This figure shows that every object has a prototype. Constructor function Foo also has its own `__proto__` which is `Function.prototype`.

So Foo is being used as a template to build objects called from it, i.e. `var Foolet = new Foo()`, while at the same, `Foo` itself uses Function.prototype as its template to build itself. *DEEP BREATH* 

So when Foo is declared for the first time, a property is created called `prototype` that points to the template that Foolet uses to build itself. So `Foolet.__proto__ === Foo.prototype`.

Let's use an example I found on StackOverflow (https://stackoverflow.com/a/42002749) to further put things in perspective:

Let's create a function:

 ```
 function Foo (name) {
  this.name = name;
 }
```

When JavaScript executes this code, it adds the `prototype` property to Foo. 

`prototype` is an object with two properties to it:

   1) `constructor`
   2) `__proto__`
   
So when we do

`Foo.prototype` it returns:

     constructor: Foo  // function definition
    __proto__: Object
    
as you can see `constructor` is nothing but the function `Foo` itself and `__proto__` points to the root level Object of JavaScript.

Now let's say we do something like this:

`var Foolet = new Foo('JavaScript');`

here's what's happening in step-by-step detail:

 1) `new Foo('JavaScript')` creates a new object in memory, then points `Foolet` to it
 2) `Foolet.__proto__` is created and points to `Foo.prototype`
 3) `Foolet` then executes `Foo.constructor()`, using itself as context for `this`, so the `'JavaScript'` argument gets passed on `Foo`'s constructor.

Note if we say `Foo.prototype.car = "BMW"` and do Foolet.car, the output "BMW" appears.

This is because when JavaScript executes this code it searches for the car property on Foolet, doesn't find it, climbs up the prototypal inheritance chain using `__proto__` (which turns out to be `Foolet.__proto__` in this case), finds it on Foo.prototype object and returns it.

Remember, `Foo.prototype` is the blueprint for `Foolet`, so `Foolet` inherits any change that is implemented on `Foo.prototype` (eg. `Foo.prototype.SomeProp = "new prop"`)

So in summary, `Foo.prototype` is a template is for all objects built from `Foo`, whereas `Foo.__proto__` points to the object Foo builds itself from(aka `Object`). So `Foo.__proto__.prototype` for example points to the prototype for all Javascript objects.

### Factory Functions

The factory function pattern is used to set up objects with the help of parameters passed in, then return them. Consider the following:

```
const personFactory = (name, age) => {
  const sayHello = () => console.log('hello!');
  return { name, age, sayHello };
};

const jeff = personFactory('jeff', 27);

console.log(jeff.name); // 'jeff'

jeff.sayHello(); // calls the function and logs 'hello!'
```

here, `jeff` is an object returned from the `personFactory` function.

below is same exact thing, but creataed with the constructor pattern instead:

```
const Person = function(name, age) {
  this.sayHello = () => console.log('hello!');
  this.name = name;
  this.age = age;
};

const jeff = new Person('jeff', 27);
```

#### Private Variables and Closure

```
const counterCreator = () => {
  let count = 0;
  return () => {
    console.log(count);
    count++;
  };
};

const counter = counterCreator();

counter(); // 0
counter(); // 1
counter(); // 2
counter(); // 3
```

The variable `count` in the function definition above is a **private variable.**  Code outside the scope of the function have no access to it.

However, the function returned _DOES_ have access to `count` variable, as well as anything elsse that would be defined in the function. This is what's called **closure.** The returned function remembers the scope in which it was defined.

### Getters and Setters

Getters and Setters are used to get/set virtual properties (properties that are 'computed' from other properties existing already in the object).

Getters and Setters can also be "smart" to filter erroneous values for properties:

```
let user = {
  get name() {
    return this._name;
  },

  set name(value) {
    if (value.length < 4) {
      alert("Name is too short, need at least 4 characters");
      return;
    }
    this._name = value;
  }
};

user.name = "Pete";
alert(user.name); // Pete

user.name = ""; // Name is too short...etc
```

The name is stored in `_name` property, and the access is done via getter and setter.

_Technically, external code is able to access the name directly by using user._name. But there is a widely known convention that properties starting with an underscore "_" are internal and should not be touched from outside the object._

An example of computed properties:

```
function User(name, birthday) {
  this.name = name;
  this.birthday = birthday;

  // age is calculated from the current date and birthday
  Object.defineProperty(this, "age", {
    get() {
      let todayYear = new Date().getFullYear();
      return todayYear - this.birthday.getFullYear();
    }
  });
}

let john = new User("John", new Date(1992, 6, 1));

alert( john.birthday ); // birthday is available
alert( john.age );      // ...as well as the age
```

# Back-End Web Architecture

## Mapping out a Request

Let's use an example of 'Alice' shopping.

1. Alice is shopping on SuperCoolShop.com. She clicks on a picture of a cover for her smartphone, and that click event makes a GET request to http://www.SuperCoolShop.com/products/66432.

Remember, GET describes the kind of request (the client is just asking for data, not changing anything). The URI (uniform resource identifier) /products/66432 specifies that the client is looking for more information about a product, and that product, has an id of 66432.

SuperCoolShop has an huge number of products, and many different categories for filtering through them, so the actual URI would be more complicated than this. But this is the general principle for how requests and resource identifiers work.

2. Alice’s request travels across the internet to one of SuperCoolShop’s servers. This is one of the slower steps in the process, because the request cannot go faster than the speed of light, and it might have a long distance to travel. For this reason, major websites with users all over the world will have many different servers, and they will direct users to the server that is closest to them!

3. The server, which is actively listening for requests from all users, receives Alice’s request!

4. Event listeners that match this request (the HTTP verb: GET, and the URI: /products/66432) are triggered. The code that runs on the server between the request and the response is called middleware.

5. In processing the request, the server code makes a database query to get more information about this smartphone case. The database contains all of the other information that Alice wants to know about this smartphone case: the name of the product, the price of the product, a few product reviews, and a string that will provide a path to the image of the product.

6. The database query is executed, and the database sends the requested data back to the server. It’s worth noting that database queries are one of the slower steps in this process. Reading and writing from static memory is fairly slow, and the database might be on a different machine than the original server. This query itself might have to go across the internet!

7. The server receives the data that it needs from the database, and it is now ready to construct and send its response back to the client. This response body has all of the information needed by the browser to show Alice more details (price, reviews, size, etc) about the phone case she’s interested in. The response header will contain an HTTP status code 200 to indicate that the request has succeeded.

8. The response travels across the internet, back to Alice’s computer.

9. Alice’s browser receives the response and uses that information to create and render the view that Alice ultimately sees!

# S.O.L.I.D Design Principe

* The **Single** Responsibility Principle - A class should have only _one_ reason to change. Example here: https://duncan-mcardle.medium.com/solid-principle-1-single-responsibility-javascript-5d9ce2c6f4a5

* The **Open/Closed** Principle - The open/closed principle says that code should be open for extension, but closed for modification. What this means is that if we want to add additional functionality, we should be able to do so simply by extending the original functionality, without the need to modify it.

* The **Liskov Substitution** Principle - The Liskov substitution principle states that any class should be substitutable for its parent class without unexpected consequences. In others words, if the classes Cat and Dog extend the class Animal, then we would expect all of the functionality held within the Animal class to behave normally for a Cat and Dog object.

* The **Interface Segregation** Principle - The interface segregation principle states that an entity should never be forced to implement an interface that contains elements which it will never use. For example, a Penguin should never be forced to implement a Bird interface if that Bird interface includes functionality relating to flying, as penguins (spoiler alert) cannot fly.

Example code:
```
class Penguin {}

class Bird {}

const flyer = {
    fly() {
        console.log(`Flap flap, I'm flying!`);
    },
};

Object.assign(Bird.prototype, flyer);

const bird = new Bird();
bird.fly(); // Outputs 'Flap flap, I'm flying!'

const penguin = new Penguin();
penguin.fly(); // 'Error: penguin.fly is not a function'
```

* The **Dependency Inversion** Principle - The dependency injection principle states that high level code should never depend on low level interfaces, and should instead use abstractions. It’s all about decoupling code. 

Example code here:

https://duncan-mcardle.medium.com/solid-principle-5-dependency-inversion-javascript-7b054685f7cb

## Object Role Stereotypes

* **Information holder** – an object designed to know certain information and provide that information to other objects.
* **Structurer** – an object that maintains relationships between objects and information about those relationships.
* **Service provider** – an object that performs specific work and offers services to others on demand.
* **Controller** – an object designed to make decisions and control a complex task.
* **Coordinator** – an object that doesn’t make many decisions but, in a rote or mechanical way, delegates work to other objects.
* **Interfacer** – an object that transforms information or requests between distinct parts of a system.

# Miscellaneous

* '--' as in `ls --all` usually denotes the long version of a switch/parameter.
  `-` as in `ls -a` denotes the short version of a switch parameter.
  
  Commands usually have one or both
  
* https://12factor.net/ - The twelve-factor app is a methodology for building software-as-a-service apps that abide by a strict set of guidelines
  
# Networking

![TypeURLInAddressBar](https://user-images.githubusercontent.com/92711276/196860787-ea770fc1-ceeb-43f1-a173-80d662c86366.jpg)

---

