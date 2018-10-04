# Weekly Review Questions

Below are some questions you might use to review and practice talking about what you've learned this week.

## General JavaScript questions
### - What does it mean that JavaScript is loosely typed?

JavaScript is a loosely typed language, meaning you don’t have to specify what type of information will be stored in a variable in advance. JavaScript automatically types a variable based on what kind of information you assign to it (e.g., that '' or " " to indicate string values). Many other languages, like Java, require you to declare a variable’s type, such as int, float, boolean, or String.

This is both a blessing and a curse.

While JavaScript’s type system gives you a lot of flexibility, it lacks the capability of a strongly typed system to yell at you when you are trying to add an int to an object, which protects you from spending hours debugging a type error.

Here are some useful tips to help you navigate JavaScript’s weird type system.

#### 1 typeof
whenever a variable’s type is in doubt, you can employ the typeof operator:

typeof 1 //> “number”
typeof “1” //> “string”
typeof [1,2,3] //> “object”
typeof {name: “john”, country: “usa”} //> “object”
typeof true //> “boolean”
typeof (1 === 1) //> “boolean”
typeof undefined //> “undefined”
typeof null //> “object”
const f = () => 2
typeof f //> “function”
Key Takeaway: When the type is in doubt, use typeof.

#### 2 Double Equals Versus Triple Equals
The === mean “equality without type coercion”. Using the triple equals, the values must be equal in type as well.

1 == “1” //> true
1 === “1” //> false
null == undefined //> true
null === undefined //> false
‘0’ == false //> true
‘0’ === false //> false
0 == false //> true
0 === false //> false, because they are of a different type
Note, you can use !== for checking inequality with type coercion:

1 != "1" //> false
1 !== "1" //> true
Key Takeaway: Always use === since that is a more thorough check and help you avoid nasty bugs.

#### 3 Checking for Falsiness
Using the ! operator (called the “bang” operator) to check for falsiness, we notice the following:

!null //> true
!undefined //> true
!0 //> true
!"" //> true
!false //> true
![] //> false
!42 //> false
!"hello world" //> false
null, undefined, 0, “”, and false all returned true when we applied the ! operator. When we applied the ! operator on things that do exist, such as the number 42 or the string “hello world”, we get false.

This shows that null, undefined, 0, “”, and false all represent non-existence or falsy values, but not in the same way. 0 is a number and represents the non-existence of quantity. “” is a string and represents the non-existence of substance in the string. And lastly, false is a boolean and represents the lack of true. This can be verified with a simple typeof:

typeof 0 //> "number"
typeof "" //> "string"
typeof false //> "boolean"
undefined and null are more tricky. As shown by the example below, they are different types, have the same value, but fails the === comparison:

typeof undefined //> “undefined”
typeof null //> “object”
undefined == null //> true
undefined === null //> false
Because null is an object, we can add a number to it. For undefinedhowever, adding a number to it gives you NaN.

null + //> 1
undefined + 1 //> NaN
Adding a number to a null seems like it could be a nice shortcut sometimes, but don’t do it!

Although null is an object, you get a number when you add a number to it. However, when you add other things to it, it gives you non-sensical strings:

null + [1,2,3]//> "null1,2,3"
null + ["hello", "world"] //> "nullhello,world"
null+ {0: "hello", 1: "world"} //> "null[object Object]"
Key Takeaways:

You need to be careful when you check for truthiness. Use === and check of all types of potential falsy values, e.g., if(foo === undefined || foo === null) ... — which can get quite tedious.
Best practice is to check for falsiness instead by using the ! operator to check for all of the falsy values, i.e., null, undefined, 0, “”, and false, in one action.
Don’t add things to null.

### - Explain some similarities and differences between `let`, `const`, and `var`.

#### var, let, or const?
A Gentle Introduction to ES6
You must understand var to grasp the benefits of let / const. Let’s rewind.

Review: Variable Declarations
It’s important to intentionally declare your variables within a specific scope, using var, to keep your code clear and maintainable.

var x = "outside";
function foo() {
  var x = "inside";
  console.log(x);
}
foo(); // inside
console.log(x); // outside
Above code properly declares x both outside and inside the function using var. What happens without var in foo?

var x = "outside";
function foo() {
  x = "inside";
  console.log(x);
}
foo(); // inside
console.log(x); // inside
Uh oh! x outside the function was overwritten by x inside the function because we didn’t specify that x was to be scoped only to foo!

Hoisting Best Practices
Declare variables using var at the top of the current scope.

OK:

console.log('sup')
var i = 0;
Better:

var i = 0;
console.log('sup')
Review: Hoisting
Variables declared using var are always hoisted to the top of their scope.

console.log(j); // ReferenceError: j is not defined
console.log(i); // undefined
var i = 0;
The variable j was never declared, so we get an error saying “I’ve never heard of j!”.

i was declared before being logged due to hoisting. Here is how the interpreter executed it:

var i;
console.log(i);
i = 0;
The interpreter moved (e.g. “hoisted”) the variable declaration to the top of the scope.

However, the variable was not assigned to 0 yet. undefined says “I know i exists, but I don’t know what value i points to because you didn’t assign it to anything”.

Supplement1: What if var refers to a function?
Supplement2: Hoisting doesn’t physically “move” your code — MDN

Function Scope
var's are function-scoped: scope is limited to the function it was defined in.

function foo() {
  var i = 0;
}
console.log(i); // ReferenceError: i is not defined
i only exists within foo so we get an error: “I’ve never heard of i!”.

Block Scope
var’s are not block-scoped: scope is not limited to the block it was defined in.

var i = 0
if (true) {
  var i = 1;
}
console.log(i); // 1
i was still in the “global scope” within the if block. i's value was overwritten, which may have not been the intention.


let
let variables are block-scoped! Specific scope = less mistakes.

let i = 0;
if (true) {
  let i = 1;
}
console.log(i); // 0
Even though i was assigned to 1 in the if block, that assignment was local to the block and therefore our “global” i was still 0. The if block’s scope was separate from the global scope.

const == Constant
const restricts over-writing variables.

const i = 0;
i = 1; // TypeError: Assignment to constant variable.
const doesn’t even let you declare a variable without assigning its (constant) value!

const i; // SyntaxError: Missing initializer in const declaration
const, like let, is block scoped.

if (true) {
  const i = 0;
}
console.log(i); // ReferenceError: i is not defined
const does allow variable mutation (only objects/arrays are mutable in JS).

Array Mutation:

const a = [1];
const b = a;
console.log(a === b); // true
b.push(2);
console.log(a === b); // true
console.log(a); /// [ 1, 2 ]
Object Mutation:

const obj = {};
obj.i = 1;
console.log(obj); // { i: 1 }
let/const Hoisting
let and const declarations are not hoisted!

EDIT: Technically they are hoisted, but they are not initialized to anything (var is initialized to undefined).
console.log(a); // undefined
var a = 2;
console.log(b); // Uncaught ReferenceError: b is not defined
console.log(c); // Uncaught ReferenceError: c is not defined
let c = 2;
This protects against variable declarations placed after references to variables.

isEqualTo5 has a bug — it always returns true.

function isEqualTo5(n) {
  return !(n - five);
  var five = 5;
}
console.log(isEqualTo5(4)); // true
The issue is that five isn’t assigned till after it’s referenced. It was declared so when it is referenced in the return statement, its value is undefined.

!(4 - undefined) === !(NaN) === true
This bug may be hard to catch. let/const to the rescue!

function isEqualTo5(n) {
  return !(n - five);
  const five = 5;
}
console.log(isEqualTo5(4)); // ReferenceError: five is not defined
const doesn’t hoist the declaration → error: reference before declaration→ prevents bug.

### - What is the difference between a variable being `undefined` and `not defined`?

`undefined` is the result of a declared variable without a value, `not defined` is the result of calling an undeclared variable

### - What is the difference between `undefined` and `null`?

`undefined` is a type, `null` is an object

### - Why might JavaScript tell you that 1 + 1 is 11 and not 2?

Because 1 and 1 are strings. When 2 strings are added they are concatenated

### - What is a template literal string?

Template literals are string literals which allow embedded expressions i.e. ``(`This is a template literal ${'embeddedExpression'}`)``

### - What is an object?

The Object constructor creates an object wrapper for the given value. If the value is null or undefined, it will create and return an empty object, otherwise, it will return an object of a Type that corresponds to the given value. If the value is an object already, it will return the value.

When called in a non-constructor context, Object behaves identically to new Object().

### - How do you access a property of an object? Is there more than one way to do this?

You can access a property of an object with `dot` or `bracket` notation.

i.e.

```var person = {};```
```person['firstname'] = 'Mario';```
```person['lastname'] = 'Rossi';```

```console.log(person.firstname);```
```// expected output: "Mario"```

```person = {'firstname': 'John', 'lastname': 'Doe'}```

```console.log(person['lastname']);```
```// expected output: "Doe"```

### - What is the difference between a function and a method?
### - Can you give some examples of objects that are built into JavaScript? 

## Arrays
### - What is an array?  
### - How do you get the first item in an array?  How do you get the last?
### - What is the difference between the Array `push()` & `pop()` methods and the `shift()` & `unshift()` methods?
### - Explain how `const` works with an array. Can you still add, remove and change values in the array? Why or why not?


## Functions
### - Can a function be assigned to a variable?  (Can a variable hold/refer to a function?)
### - What is a pure function?
### - What is the difference between procedural programming and functional programming?
### - What are some benefits of functional programming or using *pure* functions?
### - Write a function declaration for a function that adds two numbers and returns the result.
### - Write a function expression for the same function.
### - What are some differences between a function declaration and a function expression. 
### - What is an anonymous function? Give an example of where you might you use one?

## Loops
### - Write a loop to iterate through (or loop over) all the properties of an object.
### - Write a loop to iterate over all the items in an array. Can you do this more than one way? How many different ways can you think of to do this?

## Testing 
### - What is unit testing? Why is it important?
### - What is a test case? 
### - You've been asked to write a function to check that a zip code is valid.
###   - What questions might you ask to clarify this problem? 
###   - What are some test cases you might write? 

## Objects & OOP
### - What is an object?
### - How might you write an __object constructor function__ for a *Square* object?
### - How might you write a __class__ for a *Square* object?
### - What are similarities and differences between the syntax used for an object constructor function and a class?
### - What are similarities and differences between the object produced from an object constructor function and a class?
### - What is a method? How is it different from a function?
### - How do you access a property on an object?
### - How would you access an object's property using a variable for the property name?
### - Write a loop to iterate over all the properties in an object.
