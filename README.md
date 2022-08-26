# Programming-Notes
notes to reference as i learn programming and the concepts around it

# JavaScript (JS)
* When comparing values of different types, JS converts the values to numbers first
* **_null_** is its own data type and intentionally sets a value to empty. **_undefined_** is also its own type and is the default initial value for declared variables that havent been assigned anything yet. generally not good practice 
* operands of different types are converted to numbers by the equality operator ==. An empty string, just like false, becomes a zero. Ex: alert( '' == false ); // true
* But alert( '' === false ); // false because '===' is strict, instead of testing for equivalence. so it reads as "is 0 the **exact same** as false"
* When doing comparisons betweem strings, JS goes character by character.
 
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

The above says: create the function, and assign it to the variable "sayHi". 
Note the semicolon at the end, at the function expression is still part of an assignment statement

`alert( sayHi ); // shows the function code`

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
        
`document.createElement(tagName, [options])` creates a new element of tag type `tagName. [options]` in this case means you can add some optional parameters to the function.

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
 

## Node.js

Node.js is a JavaScript runtime environment that allows you to run JavaScript outside of your web browser. 

# Miscellaneous

* '--' as in `ls --all` usually denotes the long version of a switch/parameter.
  `-` as in `ls -a` denotes the short version of a switch parameter.
  
  Commands usually have one or both
