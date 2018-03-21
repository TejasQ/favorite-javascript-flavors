# Tejas' Favorite JavaScript Flavors 🍫🥓🍕

JavaScript is such a diverse language that allows a myriad of dialects and code styles. It _can be_ very welcoming to newcomers, while also staying interesting and even challenging to more senior engineers. As such, I thought I'd document a few of my favorite "flavors" of JavaScript that have the capacity to really help fellow engineers.

## 🍫 Preferred Flavors

Below is my preferred style of writing JavaScript and _the reasons_ that go with it.

### [Functional](https://en.wikipedia.org/wiki/Functional_programming)

In JavaScript, functions are called _"first-class citizens"_. That's just a fancy way of saying that they can be passed around like variables. Consider,

```js
// Look, variables can be passed around!
var x = 2;
var y = x; // 2

// Look, functions can be passed around!
function a() {
  return true;
}

var b = a; // still a reference to the function, that can later be called like

b(); // returns true, just as a(); would.
```

Some of the benefits of writing code functionally, as opposed to in an object-oriented (or class-based) style, are:

* Since functions are first-class citizens, they can be reused in multiple contexts to reliably _do their jobs_ given the right parameters.
* (Pure) functions are stateless and _don't care about state_, but just take inputs and return output and are therefore safer.
* Because of the previous two points, a functional style promotes good principles like [modular design](https://en.wikipedia.org/wiki/Modular_design), [separation of concerns](https://en.wikipedia.org/wiki/Separation_of_concerns), and [immutability](https://github.com/TejasQ/favorite-javascript-flavors#immutable).

### [Declarative](https://en.wikipedia.org/wiki/Declarative_programming)

Declarative code is basically code that describes _what_ to do, as opposed to _how_ to do it. Consider a simple problem of incrementing each array element by a number:

```js
const addToArray = (array, numberToAdd) => array.map(arrayItem => arrayItem + numberToAdd);

const myArray = [1, 2, 3];
addToArray(myArray, 3); // [4, 5, 6];
```

We can already see how the first-class nature of functions comes in handy here: the thing inside `array.map()` is a function!

The advantage of declarative code (which is usually an abstraction on top of some imperative code) is that it plain and simply _declares_ an action, instead of describing a _course of action_, allowing more terse code that can be clearly tested for functionality: `doThat()` can be more clearly tested than `for (var _i; _i < something; _i++) { /* do something */ }`.

Here's an imperative example of the previous function:

```js
function addToArray(array, numberToAdd) {
  let result = [];
  for (let _i = 0; _i < array.length; _i++) {
    result[_i] = array[_i] + numberToAdd;
  }
  return result;
}

const myArray = [1, 2, 3];
addToArray(myArray, 3); // [4, 5, 6];
```

Can we see the difference in readability and comprehensibility? It is quite literally saying _ok, here's how I'll do it_, instead of _do this_. Declarative code also better promotes thinking in terms of [encapsulation](<https://en.wikipedia.org/wiki/Encapsulation_(computer_programming)>), [minimal privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege) and [single responsibility](https://en.wikipedia.org/wiki/Single_responsibility_principle) of software components.

### [Immutable](https://en.wikipedia.org/wiki/Immutable_object)

Immutable data structures are data structures that, by design, _cannot_ be changed: like strings in JavaScript. Immutable-style programming is a similar concept that condemns mutation of values throughout the application. Consider,

```js
const myName = "Tejas";
let myLastName = "Kumar";

// 200 lines later, where we want to change myName for whatever reason...
myName = "Bob"; // <- will cause an error because myName is immutable.
myLastName = "Tony"; // <- will change this variable that may be referenced elsewhere, causing things to break.
```

Perhaps a more robust example is working with objects:

```js
const myThing = {
  isThing: true,
  name: "Tejas"
};

// later...
delete myThing.name; // <- mutated myThing

// 2 years later...
console.log("Hello, ", myThing.name); // <- broken: "Hello, undefined"
```

An immutable flavor of this would look like:

```js
const myThing = {
  isThing: true,
  name: "Tejas"
};

// later...
const { name, ...myNewThing } = myThing;
```

That basically creates a new object, `myNewThing`, omitting the name property using [ES6's object rest-spread operator](https://github.com/tc39/proposal-object-rest-spread). This prevents unexpected behavior in an application from values suddenly changing over time, or ceasing to be defined and is thus considered a safer practice.

## 🕷 Other Flavors

I'm not too enthusiastic about these patterns/styles in JavaScript.

### [Object-Oriented/Class-Based](https://en.wikipedia.org/wiki/Class-based_programming)

JavaScript does not contain native support for classes. There, I said it. JavaScript contains syntactic sugar and a `class` keyword, even an `extends` keyword to model inheritance, but these break down under to hood to... functions and objects. Consider,

```js
class myClass {
  constructor() {
    this._privateThing = "hahaha";
  }
  eat(thingToEat) {
    return "Munch munch munch " + thingToEat;
  }
  eatThePrivateThing() {
    this.eat(this._privateThing);
  }
}
```

That's cool, but basically under the hood, this equates to:

```js
var myClass = (function() {
  function myClass() {
    this._privateThing = "hahaha";
  }
  myClass.prototype.eat = function(thingToEat) {
    return "Munch munch munch " + thingToEat;
  };
  myClass.prototype.eatThePrivateThing = function() {
    this.eat(this._privateThing);
  };
  return myClass;
})();
```

Functions and objects. _sigh_

The reasons that I do not prefer (or advocate) this style of JavaScript are because:

* It is a lie.
* It promotes thinking Java-style, but this is not Java.
* More concretely, it promotes thinking in terms of [inheritance](<https://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)>) over more [functional composition](https://en.wikipedia.org/wiki/Object_composition).
  * Composing things together allows higher flexibility.
  * Composition allows creating components that _work together_ instead of trying to find commonalities between pre-existing _things_.
    * For example, a _car_ has an _exhaust_ and a _wheel_: we can say a car is a composition of those parts, that work faithfully, or better yet, a car _**has an**_ exhaust and a wheel.
    * The object-oriented version of this would be more based around a model of a Car, that _**is a**_ _thing_ that comes with an exhaust and a wheel: the exhaust and wheel _are parts of the car_ and thus, are tightly coupled to it and cannot be independently modularized, run, tested, or composed with other parts to make a... say an _airplane_ or _cruise ship_, both of which _also_ have wheels and exhausts.
      * This is my chief issue with inheritance: it presents the [gorilla-banana problem](https://www.johndcook.com/blog/2011/07/19/you-wanted-banana/).

### [Imperative](https://en.wikipedia.org/wiki/Imperative_programming)

Imperative code is basically code that describes _how_ to do an operation, as opposed to _what_ is being done. Typically, one would find imperative code under a more declarative abstraction. Consider a case where one would want to find the first odd number in a list of numbers:

```js
const numbers = [4, 6, 10, 12, 13, 14, 15, 16, 17];
let result;
for (let _i = 0; _i < numbers.length; _i++) {
  if (numbers[_i] % 2 === 1) {
    result = numbers[_i];
    break;
  }
}
```

That approach basically says

> okay, I know what we need to do! Here's how we'll do it: we'll iterate through each one of them and then check if it has a 1 remainder when divided by 2 and then store it in a variable.

In other words, it says _how to do it_. Cool, but I prefer a more declarative abstraction:

```js
const numbers = [4, 6, 10, 12, 13, 14, 15, 16, 17];
const result = numbers.find(number => number % 2 === 1);
```

The above example says

> here's what to do: find the number that has a 1 remainder when divided by 2 and then gimme it.

In other words, it says _what_ to do. In large codebases, as a developer, I tend to appreciate reading through the logic of a computation without being concerned with its control flow.

### Mutable

I won't say much about this. Basically, if code is being mutated throughout a codebase, it can be a potential nightmare to find out what changed where and caused what bug and how and why and _aipegaepgjaepgjaepgaeg_.

## Conclusion

I hope you've enjoyed reading the kind of JavaScript style I prefer. I basically wrote this and put it here as a way of sharing it with people who ask me because I love them and I try to be DRY and stuff. _kthxbai_ <3
