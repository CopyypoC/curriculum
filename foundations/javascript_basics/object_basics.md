### Introduction

Congratulations on making it to one of the last lessons in Foundations! By this point, you have learned many of the fundamentals of JavaScript. In this lesson, you will learn about Objects - a collection of key-value pairs - as well as some more powerful and commonly used array methods.

Before you know it, you'll have a better understanding of how powerful objects and arrays are and how both can be an indispensable part of your JavaScript tool kit!

### Lesson overview

This section contains a general overview of topics that you will learn in this lesson.

- Creating objects.
- Accessing object properties.
- Using multiple object operators.
- Understanding the differences between object and primitive data types.
- Using the `map`, `filter` and `reduce` array methods.

### Objects

Objects are a *very* important part of the JavaScript language, and while for the most part you can accomplish simple and even intermediate tasks without worrying about them, any real project that you're going to attempt is going to feature Objects. The uses of Objects in JavaScript can get deep relatively quickly, so for the moment we're only going to cover the basics. There'll be an in-depth dive later.

1. This JavaScript.info [article on objects](https://javascript.info/object) is the best place to get started.
1. The [MDN tutorial on objects](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Basics) isn't bad either, so check it out if you need another take on the subject.

### Differences between objects and primitives

Earlier in the curriculum you've learned about [primitive data types](https://www.theodinproject.com/lessons/foundations-data-types-and-conditionals). Now that you've seen the object data type, which includes but is not limited to, objects ({key: value}), arrays, and functions. The main difference between the two is that primitives can contain only a single thing (string, number, etc). Objects data types are used to store a collection of data and more complex entities.

Besides the formal differences, there are also some technical differences which affect how we use each data type. When you define a primitive variable, it will contain a copy of the information provided to it:

```javascript
let data = 42;
// dataCopy will store a copy of what data contains, so a copy of 42
let dataCopy = data;

// which means that making changes to dataCopy won't affect data
dataCopy = 43;

console.log(data); // 42
console.log(dataCopy); // 43
```

On the other hand, when you define an object variable, it will contain a *reference* to the object provided to it:

```javascript
// obj contains a reference to the object we defined on the right side
const obj = { data: 42 };
// objCopy will contain a reference to the object referenced by obj
const objCopy = obj;

// making changes to objCopy will make changes to the object that it refers to
objCopy.data = 43;

console.log(obj); // { data: 43 }
console.log(objCopy); // { data: 43 }
```

This behavior isn't new to you. In your last project, you made changes to the cells in the Etch-A-Sketch grid by using references. Let's take this code snippet as an example:

```javascript
const element = document.querySelector("#container");
element.style.backgroundColor = "red";
```

We're mutating the variable we declared (`element`), yet the changes affect the corresponding node in the DOM. Why does it happen? That's because the node we have in our code is a **reference** to the same node that our DOM uses. If that wasn't a reference, but a copy like primitive data types behave, our changes would have **no** effect! Because the changes would be made to the local copy we have.

This behavior is also something to consider when we pass arguments to a function. Let's take the following functions for example:

```javascript
function increaseCounterObject(objectCounter) {
  objectCounter.counter += 1;
}

function increaseCounterPrimitive(primitiveCounter) {
  primitiveCounter += 1;
}

const object = { counter: 0 };
let primitive = 0;

increaseCounterObject(object);
increaseCounterPrimitive(primitive);
```

Take a moment and guess what will happen to `object` and what will happen to `primitive` after we make the function calls.

If you answered that the object counter would increase by 1, and the primitive counter wouldn't change, you're correct. Remember that the parameter `objectCounter` contains a *reference* to the same object as the `object` variable we gave it, while `primitiveCounter` only contains a copy of the primitive value.

<div class="lesson-note" markdown="1">

#### Reassigning object data type variables

While mutating the object we have a reference to affects all other variables that reference it, reassigning a variable does not change what the other variables refer to. For example:

```javascript
let animal = { species: "dog" };
let dog = animal;

// reassigning animal variable with a completely new object
animal = { species: "cat" };

console.log(animal); // { species: "cat" }
console.log(dog); // { species: "dog" }
```

</div>

### Intermediate/advanced array magic

Besides being a quick and handy way to store data, arrays also have a set of functions for manipulating that data in very powerful ways. Once you begin to master these functions you will start to see ways to use them all over the place! There are really only a handful of these functions, but as you'll soon see, the possibilities of what you can do with them are near endless.

As an example of what we mean, let's consider a `sumOfTripledEvens` function. It will:

1. Take in an array.
1. For every even number, it will triple it.
1. Then it will sum all those even numbers.

Can you try thinking of how you could implement a function like that using pseudocode?

1. We need to perform an operation only on the even numbers.
1. We need to transform *those* numbers by multiplying them by 3.
1. Finally, we need to add the result up from the previous transformation.

So using that logic, you may end up implementing something like this:

```javascript
function sumOfTripledEvens(array) {
  let sum = 0;
  for (let i = 0; i < array.length; i++) {
    // Step 1: If the element is an even number
    if (array[i] % 2 === 0) {
      // Step 2: Multiply this number by three
      const tripleEvenNumber = array[i] * 3;

      // Step 3: Add the new number to the total
      sum += tripleEvenNumber;
    }
  }
  return sum;
}
```

In the above code, there are 3 important snippets to consider:

- `if (array[i] % 2 === 0)`: checks if a given number is even.
- `array[i] * 3;`: multiply this number by three.
- `sum += tripleEvenNumber`: add that number to the total.

Every single piece solves a crucial problem with our code.
But it is possible to make the function more concise and readable by using some built-in array methods.
These methods are slightly more complicated than you've been used to, so let's take a moment to understand how to use them.

#### The map method

`map` is one such function. It expects a `callback` as an argument, which is a fancy way to say "I want you to pass another function as an argument to my function".

Let's say we had a function `addOne`, which takes in `num` as an argument and outputs that `num` increased by 1.
And let's say we had an array of numbers, `[1, 2, 3, 4, 5]` and we'd like to increment all of these numbers by 1 using our `addOne` function.
Instead of making a `for` loop and iterating over the above array, we could use our `map` array method instead, which **automatically** iterates over an array for us.
We don't need to do any extra work aside from simply passing the function we want to use in:

```javascript
function addOne(num) {
  return num + 1;
}
const arr = [1, 2, 3, 4, 5];
const mappedArr = arr.map(addOne);
console.log(mappedArr); // Outputs [2, 3, 4, 5, 6]
```

`map` returns a new array and does not change the original array.

```javascript
// The original array has not been changed!
console.log(arr); // Outputs [1, 2, 3, 4, 5]
```

Using `map` in this way is much more elegant than writing a `for` loop and iterating over the array. But we can do even better. We can define an inline function right inside of `map` like so:

```javascript
const arr = [1, 2, 3, 4, 5];
const mappedArr = arr.map((num) => num + 1);
console.log(mappedArr); // Outputs [2, 3, 4, 5, 6]
```

#### The filter method

`filter` is somewhat similar to `map`. It still iterates through the array and applies the callback function on every item. However, instead of transforming the values in the array, it returns the original values of the array, but only IF the callback function returns `true`.
Let's say we had a function, `isOdd` that returns either `true` if a number is odd or `false` if it isn't.

The `filter` method expects the `callback` to return either `true` or `false`. If it returns `true`, the value is included in the output. Otherwise, it isn't.
Consider the array from our previous example, `[1, 2, 3, 4, 5]`.
If we wanted to remove all even numbers from this array, we could use `.filter()` like this:

```javascript
function isOdd(num) {
  return num % 2 !== 0;
}
const arr = [1, 2, 3, 4, 5];
const oddNums = arr.filter(isOdd);
console.log(oddNums); // Outputs [1, 3, 5];
console.log(arr); // Outputs [1, 2, 3, 4, 5], original array is not affected
```

- `filter` will iterate through `arr` and pass **every value** into the `isOdd` callback function, one at a time.
- `isOdd` will return `true` when the value is odd, which means this value is included in the output.
- If it's an even number, `isOdd` will return `false` and not include it in the final output.

#### The reduce method

Finally, let's say that we wanted to multiply all of the numbers in our `arr` together like this: `1 * 2 * 3 * 4 * 5`.
First, we'd have to declare a variable `total` and initialize it to 1. Then, we'd iterate through the array with a `for` loop and multiply the `total` by the current number.

But we don't actually need to do all of that, we have our `reduce` method that will do the job for us. Just like `.map()` and `.filter()` it expects a callback function.
However, there are two key differences with this array method:

- The callback function takes two arguments instead of one. The first argument is the `accumulator`, which is the current value of the result *at that point in the loop*. The first time through, this value will either be set to the `initialValue` (described in the next bullet point), or the first element in the array if no `initialValue` is provided. The second argument for the callback is the `current` value, which is the item currently being iterated on.
- It also takes in an `initialValue` as a second argument (after the callback), which helps when we don't want our initial value to be the first element in the array. For instance, if we wanted to sum all numbers in an array, we could call reduce without an `initialValue`, but if we wanted to sum all numbers in an array and add 10, we could use 10 as our `initialValue`.

```javascript
const arr = [1, 2, 3, 4, 5];
const productOfAllNums = arr.reduce((total, currentItem) => {
  return total * currentItem;
}, 1);
console.log(productOfAllNums); // Outputs 120;
console.log(arr); // Outputs [1, 2, 3, 4, 5]
```

In the above function, we:

- Pass in a callback function, which is `(total, currentItem) => { return total * currentItem }`.
- Initialize total to `1` in the second argument.

So what `.reduce()` will do is go through every element in `arr` and apply the `callback` function to it. It updates `total` without actually changing the array itself. After it’s done, it returns `total`.

#### Summary

You've learnt about the three powerful array methods which are `map`, `filter` and `reduce`. They allow us to write shorter code that is more readable and less prone to bugs.

For a quick recap of these array methods, consider this picture which should visually explain them in terms of sandwiches:

![example of filter, map and reduce methods being used to visually represent making a sandwich](https://static.observableusercontent.com/thumbnail/bea194824f0d5842addcb7910bb488795c6f80f143ab5332b28a317ebcecd603.jpg)

Let's do some quick practice before your assignment! Rewrite the `sumOfTripledEvens(array)` function using these three methods.

Once you are finished and you've tested that your function works correctly, check out the solution below.

<details markdown="block"><summary>Solution</summary>

```javascript
function sumOfTripledEvens(array) {
  return array
    .filter((num) => num % 2 === 0)
    .map((num) => num * 3)
    .reduce((acc, curr) => acc + curr);
}
```

</details>

### Assignment

<div class="lesson-content__panel" markdown="1">

1. Read through the [array method guide](https://javascript.info/array-methods) for a comprehensive overview of array methods in JavaScript. Complete the exercises at the end, except for "Create an extendable calculator," as it involves more advanced concepts that we have not yet covered.
1. Follow up by watching [JavaScript Array Cardio Practice - Day 1](https://www.youtube.com/watch?v=HB1ZC7czKRs) by Wes Bos. To follow along, fork and clone the [JavaScript30 repository](https://github.com/wesbos/JavaScript30).
1. Watch and code along with [Array Cardio Day 2](https://www.youtube.com/watch?v=QNmRfyNg1lw).
1. At this point you just need a little more practice! Go back to the [JavaScript exercises repository](https://github.com/TheOdinProject/javascript-exercises) that we introduced in the [Arrays and Loops](https://www.theodinproject.com/lessons/foundations-arrays-and-loops) assignment. Review each README file prior to completing the following exercises in order:
   - `08_calculator`
   - `09_palindromes`
   - `10_fibonacci`
   - `11_getTheTitles`
   - `12_findTheOldest`

Note: Solutions for these exercises can be found in the `solution` folder of each exercise.

If you feel yourself getting overwhelmed or stuck, don't be afraid to go back and review or ask for help in the [TOP Discord server](https://discord.com/invite/theodinproject)!

</div>

### Knowledge check

The following questions are an opportunity to reflect on key topics in this lesson. If you can't answer a question, click on it to review the material, but keep in mind you are not expected to memorize or master this knowledge.

- [What is the difference between objects and arrays?](https://javascript.info/object#summary)
- [How do you access object properties?](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Basics#bracket_notation)
- [How do primitives and object types differ when you assign them to other variables, or pass them into functions?](https://www.theodinproject.com/lessons/foundations-object-basics#differences-between-objects-and-primitives)
- [What is `Array.prototype.map()` useful for?](https://www.youtube.com/watch?v=HB1ZC7czKRs&t=233s)
- [What is `Array.prototype.filter()` useful for?](https://www.youtube.com/watch?v=HB1ZC7czKRs&t=84s)
- [What is `Array.prototype.reduce()` useful for?](https://youtu.be/HB1ZC7czKRs?t=467)

### Additional resources

This section contains helpful links to related content. It isn't required, so consider it supplemental.

- Learn about the `arguments` object on this [page about function parameters](https://www.w3schools.com/js/js_function_parameters.asp).
