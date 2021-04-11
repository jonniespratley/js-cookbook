---
description: JavaScript things you should know about.
---

# Things to know

## Functions

All about functions

* `.apply()` - Calls a function and sets its this to the provided value, arguments can be passed as an Array object.
* `.bind()`- Creates a new function which, when called, has its this set to the provided value, with a given sequence of arguments preceding any provided when the new function was called.
* `Function.prototype.call()` - Calls \(executes\) a function and sets its this to the provided value, arguments can be passed as they are.

#### Ways to define a function:

```javascript
//function delcation
function foo() {
  console.log(arguments);
}   

//expression
const foo = function() {
  console.log(arguments);
}
foo; // ‘undefined'
foo(); // this raises a TypeError
const foo = function() {};

//named function expression
var foo = function bar() {
    bar(); // Works
}
bar(); // ReferenceError


function foo(a, b, c) {}

var bar = {};
foo.apply(bar, [1, 2, 3]); // array will expand to the below
foo.call(bar, 1, 2, 3); // results in a = 1, b = 2, c = 3
```



#### ES5 Functions

```javascript
function Foo() {}
Foo.prototype.method = function() {
  console.log(2, this);
};

function Bar() {}
Bar.prototype = Foo.prototype;

new Foo().method();
new Bar().method();
```

#### ES6 Functions

```javascript
//ES6
class Foo {
  method() {
    console.log(this.name);
  }
}

class Bar extends Foo {
  constructor(){
    super();
  }
}


new Bar().method();
```

#### Async Functions

```javascript
// Async function declaration
async function func1() {}

// Async function expression
const func2 = async function () {};

// Async arrow function
const func3 = async () => {};

// Async method definition (in classes, too)
const obj = { async m() {} };
```



 **.bind\(\): pre-filling this and parameters of functions**

Therefore, `.bind()` can be implemented as a real function as follows:

```text
const boundFunc = someFunc.bind(thisValue, arg1, arg2, arg3);
```

That is, the following two function calls are equivalent:

```javascript
boundFunc('a', 'b')
someFunc.call(thisValue, arg1, arg2, arg3, 'a', 'b')
```

Another way of pre-filling this and parameters, is via an arrow function:

```javascript
const boundFunc2 = (...args) =>
  someFunc.call(thisValue, arg1, arg2, arg3, ...args);
```

Therefore, `.bind()` can be implemented as a real function as follows:

```javascript
function bind(func, thisValue, ...boundArgs) {
  return (...args) =>
    func.call(thisValue, ...boundArgs, ...args);
}
```

Creates a function that invokes `func` with a given context, optionally adding any additional supplied parameters to the beginning of the arguments.

```javascript
const bind = (fn, context, ...boundArgs) => (...args) => fn.apply(context, [...boundArgs, ...args]);
```

#### \*\*\*\*

#### **call and apply, why use and the benefits and differences**

The Apply and Call methods are two of the most often used Function methods in JavaScript, and for good reason: they allow us to borrow functions and set the this value in function invocation.

```text
let args = [1, 2, 3];

func.call(context, ...args); // pass an array as list with spread operator
func.apply(context, args);   // is same as using apply

let wrapper = function() {
  return anotherFunction.apply(this, arguments);
};
```

In addition, the apply function in particular allows us to execute a function with an array of parameters, such that each parameter is passed to the function individually when the function executes—great for variadic functions; a variadic function takes varying number of arguments, not a set number of arguments as most functions do.



#### Currying

Currying is translating a function from callable as f\(a, b, c\) into callable as f\(a\)\(b\)\(c\).

```javascript
function curry(func) {
  return function(a) {
    return function(b) {
      return func(a, b);
    };
  };
}

//es6
function curry(func) {
  return a => b => func(a, b);
}

// usage
function sum(a, b) {
  return a + b;
}

let carriedSum = curry(sum);

alert( carriedSum(1)(2) ); // 3
```

As you can see, the implementation is a series of wrappers.

* The result of `curry(func)` is a wrapper function\(a\).
* When it is called like `sum(1)`, the argument is saved in the Lexical Environment, and a new wrapper is returned function\(b\).
* Then `sum(1)(2)` finally calls function\(b\) providing 2, and it passes the call to the original multi-argument sum.

#### DOM Library

Be able to create wrapper for dom library, for example:

```javascript
const Library = function(params) {
      const selector = document.querySelectorAll(params);
      this.length = selector.length;
      for (let count = 0; count < this.length; count++) {
         this[count] = selector[count];
      }
      return params;
   };
```

#### Closures

A closure is an inner function that has access to the outer \(enclosing\) function's variables—scope chain. The closure has three scope chains: it has access to its own scope \(variables defined between its curly brackets\), it has access to the outer function's variables, and it has access to the global variables.

The inner function has access not only to the outer function’s variables, but also to the outer function’s parameters. Note that the inner function cannot call the outer function’s arguments object, however, even though it can call the outer function’s parameters directly.

You create a closure by adding a function inside another function.

A Basic Example of Closures in JavaScript:

```javascript
function showName (firstName, lastName) { 
    var nameIntro = "Your name is ";

    // this inner function has access to the outer function's variables, including the parameter
    function makeFullName () {         
        return nameIntro + firstName + " " + lastName;     
    }

    return makeFullName(); 
} 

showName ("Michael", "Jackson"); // Your name is Michael Jackson 
```

## Arrays

**Mutator methods**

These methods modify the array:

| Method | Description |
| :--- | :--- |
| copyWithin | Copies a sequence of array elements within the array. |
| fill | Fills all the elements of an array from a start index to an end index with a static value. |
| pop | Removes the last element from an array and returns that element. |
| push | Adds one or more elements to the end of an array and returns the new length of the |
| reverse | Reverses the order of the elements of an array in place — the first becomes the last, and the last becomes the first. |
| shift | Removes the first element from an array and returns that element. |
| sort | Sorts the elements of an array in place and returns the array. |
| splice | Adds and/or removes elements from an array. |
| unshift | Adds one or more elements to the front of an array and returns the new length of the array. |

#### **Accessor methods**

These methods do not modify the array and return some representation of the array.

| Method | Description |
| :--- | :--- |
| concat | Returns a new array comprised of this array joined with other array\(s\) and/or value\(s\). |
| includes | Determines whether an array contains a certain element, returning true or false as appropriate. |
| indexOf | Determines whether an array contains a certain element, returning true or false as appropriate. |
| join | Returns the first \(least\) index of an element within the array equal to the specified value, or -1 if none is found. |
| lastIndexOf | Returns the last \(greatest\) index of an element within the array equal to the specified value, or -1 if none is found. |
| slice | Extracts a section of an array and returns a new array. |
| toSource | Returns an array literal representing the specified array; you can use this value to create a new array. Overrides the Object.prototype.toSource\(\) method. |

  
**Iteration Methods**

| Method | Description |
| :--- | :--- |
| entries | Returns a new Array Iterator object that contains the key/value pairs for each index in the array. |
| every | Returns true if every element in this array satisfies the provided testing function. |
| filter | Creates a new array with all of the elements of this array for which the provided filtering function returns true. |
| find | Returns the found value in the array, if an element in the array satisfies the provided testing function or undefined if not found. |
| findIndex | Returns the found index in the array, if an element in the array satisfies the provided testing function or -1 if not found. |
| forEach | Calls a function for each element in the array. |
| keys | Returns a new Array Iterator that contains the keys for each index in the array. |
| map | Creates a new array with the results of calling a provided function on every element in this array. |
| reduce | Apply a function against an accumulator and each value of the array \(from left-to-right\) as to reduce it to a single value. |
| reduceRight | Apply a function against an accumulator and each value of the array \(from right-to-left\) as to reduce it to a single value. |
| some | Returns true if at least one element in this array satisfies the provided testing function. |
| values | Returns a new Array Iterator object that contains the values for each index in the array. |

### **Strings**

* indexOf -
* charAt\(index\) -
* Slice\(\)  - Extracts a section of a string and returns a new string.
* Split\(\) - Splits a String object into an array of strings by separating the string into substrings
* startsWith - Determines whether a string begins with the characters of another string.Substring - Returns the characters in a string between two indexes into the string.

#### Stacks

LIFO - last in first 

```javascript
//Stack with Array - First in, Last out
const stack = []; 
stack.push(2); // stack is now [2] 
stack.push(5); // stack is now [2, 5] 

const i = stack.pop(); // stack is now [2] 
console.log(i); // displays 5 
```

#### Queues

FIFO - first in first out

```javascript
const queue = []; 
queue.push(2); // queue is now [2] 
queue.push(5); // queue is now [2, 5] 
queue.push(7); // queue is now [2, 5, 7] 

const i = queue.shift(); // queue is now [5, 7]
console.log(i); // displays 2
```

### Big O Notation

 Take the following examples and identify the big-O;

| **Big O**  | **X for 10 elements** | **X for 100 elements** | **X for 1000 elements** |
| :--- | ---: | ---: | ---: |
| **O\(1\)** | 1 | 1 | 1 |
| **O\(log N\)** | 3 | 6 | 9 |
| **O\(N\)** | 10 | 100 | 1000 |
| **O\(N log N\)** | 30 | 600 | 9000 |
| **O\(N^2\)** | 100 | 10000 | 1000000 |
| **O\(2^N\)** | 1024 | 1.26e+29 | 1.07e+301 |
| **O\(N!\)** | 3628800 | 9.3e+157 | 4.02e+2567 |

For examples in JavaScript see below:

```javascript
// O(n)
function fn1(n) { 
  for (let i = 0; i < n; i++) { 
    console.log(i);
  }
}
fn1(5);

// O(n^2)
function fn2(n) { 
    let times = 0; 
    for (let i = 0; i < n; i++) { 
        times++;
        for (let j = 0; j < n; j++) { 
            times++
            console.log(j);
        }
    }
    console.log('fn2', n, times)
}
fn2(5);


// O( log(n) )
function fn3(n) { 
    let times = 0; 
    for (let i = 0; i < n; i++) { 
        times++;
        for (let j = i + 1; j < n; j++) { 
            times++;
            console.log(i + '-' + j); 
        }
    }
    console.log('fn3', n, times)
}
fn3(5);


function fn4(n) { 
    let times = 0; 
    for (let i = 1; i <= n; i *= 2) { 
        times++
        console.log(i);
    }
    console.log('fn4', n, times)
}
fn4(5);


function fn5(n) { 
    if (n === 0) return; 
    console.log(n);
    fn5(n - 1); 
}
fn5(5);
```

**Data Structure Operations Complexity**

The following data structures and the complexity it takes for operations on them.

| **Data Structure** | **Access** | **Search** | **Insertion** | **Deletion** | **Comments** |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Array** | 1 | n | n | n |  |
| **Stack** | n | n | 1 | 1 |  |
| **Queue** | n | n | 1 | 1 |  |
| **Linked List** | n | n | 1 | 1 |  |
| **Hash Table** | - | n | n | n | In case of perfect hash function costs would be O\(1\) |
| **Binary Search Tree** | n | n | n | n | In case of balanced tree costs would be O\(log\(n\)\) |
| **B-Tree** | log\(n\) | log\(n\) | log\(n\) | log\(n\) |  |
| **Red-Black Tree** | log\(n\) | log\(n\) | log\(n\) | log\(n\) |  |
| **AVL Tree** | log\(n\) | log\(n\) | log\(n\) | log\(n\) |  |
| **Bloom Filter** | - | 1 | 1 | - | False positives are possible while searching |

#### Array Sorting Complexity

The following table is about array sorting times.

| **Name** | **Best** | **Average** | **Worst** | **Memory** | **Stable** | **Comments** |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Bubble sort** | n | n2 | n2 | 1 | Yes |  |
| **Insertion sort** | n | n2 | n2 | 1 | Yes |  |
| **Selection sort** | n2 | n2 | n2 | 1 | No |  |
| **Heap sort** | n log\(n\) | n log\(n\) | n log\(n\) | 1 | No |  |
| **Merge sort** | n log\(n\) | n log\(n\) | n log\(n\) | n | Yes |  |
| **Quick sort** | n log\(n\) | n log\(n\) | n2 | log\(n\) | No | Quicksort is usually done in-place with O\(log\(n\)\) stack space |
| **Shell sort** | n log\(n\) | depends on gap sequence | n \(log\(n\)\)2 | 1 | No |  |
| **Counting sort** | n + r | n + r | n + r | n + r | Yes | r - biggest number in array |
| **Radix sort** | n \* k | n \* k | n \* k | n + k | Yes | k - length of longest key |

