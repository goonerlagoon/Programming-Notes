# Programming-Notes
notes to reference now and in the future as i learn programming and the concepts around it

# JavaScript (JS)
* When comparing values of different types, JS converts the values to numbers first
* _null_ is its own data type and intentionally sets a value to empty. _undefined_ is also its own type and is the default initial value for declared variables that havent been assigned anything yet. generally not good practice 
* operands of different types are converted to numbers by the equality operator ==. An empty string, just like false, becomes a zero. Ex: alert( '' == false ); // true
* But alert( '' === false ); // false because '===' is strict, instead of testing for equivalalaence
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
   
* The number 0, an empty string "", null, undefined, and NaN are all falsy values. Everything else is truthy.
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
