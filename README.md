# Programming-Notes
notes to reference now and in the future as i learn programming and the concepts around it

# JavaScript (JS)
* When comparing values of different types, JS converts the values to numbers first
* operands of different types are converted to numbers by the equality operator ==. An empty string, just like false, becomes a zero. Ex: alert( '' == false ); // true
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


