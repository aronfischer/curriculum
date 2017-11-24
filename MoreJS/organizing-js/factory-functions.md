# Factory Functions and Modules

## What's wrong with constructors

As mentioned in the introduction there are _many_ ways of organizing your code.   Object constructors are but one way to do that!  They are fairly common in the wild and are a fundamental building block of the JavaScript language.

_However..._

There are many people who argue _against_ using constructors at all.  There are a few arguments that are made but it basically comes down to the fact that if you aren't careful, it can be easy to introduce bugs into your code when using constructors.  [This](https://tsherif.wordpress.com/2013/08/04/constructors-are-bad-for-javascript/) article does a pretty decent job of outlining the issues. (spoiler alert => the author ends up recommending Factory Functions)

One of the biggest issues with constructors is that while they _look_ just like regular functions, they do not behave like regular functions at all.  If you try to use a constructor function with out the `new` keyword, your program will not work as expected, but you won't find any obvious errors when you run the code either.

The main take-away here is that while constructors aren't necessarily _evil_, but they aren't the only way and they may not be the best way either.  Of course this doesn't mean that time learning about them was wasted!  They are a common pattern in real-world code and many tutorials that you'll come across on the net.

## Factory Function introduction

The factory function pattern is very similar to the constructor pattern, but instead of using the `new` keyword to create an object, factory functions simply set up and return the new object when you call the function.  Check out this example.

```javascript
const personFactory = (name, age) => {
  const sayHello = () => console.log('hello!');
  return { name, age, sayHello };
};

const jeff = personFactory('jeff', 27);

console.log(jeff.name); // 'jeff'

jeff.sayHello(); // calls the function and logs 'hello!'
```

for reference, here is the same thing created using the Constructor pattern:

```javascript
const Person = function(name, age) {
  this.sayHello = () => console.log('hello!');
  this.name = name;
  this.age = age;
};

const jeff = new Person('jeff', 27);
```

### Object Shorthand

A quick note about line 3 from the factory function example.  In 2015 a handy new shorthand for creating objects was added into JavaScript.  Without the shorthand line 3 would have looked something like this:

```javascript
return {name: name, age: age, sayHello: sayHello}
```

Put simply, if you are creating an object where you are referring to a variable that has the exact same name as the object property you're creating you can condense it and leave out the repetition:

```javascript
return {name, age, sayHello}
```

With that knowledge in your pocket, check out this little hack:

```javascript
const name = "Maynard"
const color = "red"
const number = 34
const food = "rice"

// logging all of these variables might be a useful thing to do,
// but doing it like this can be somewhat confusing.
console.log(name, color, number, food) // Maynard red 34 rice

// if you simply turn them into an object with brackets,
// the output is much easier to decipher:
console.log({name, color, number, food}) 
	// { name: 'Maynard', color: 'red', number: 34, food: 'rice' }
```

## Scope and Closure

From simply reading the above example you are probably already in pretty good shape to start using factory functions in your code, but before we get there it's time to do a somewhat deep dive into a very important concept in intermediate to advanced JavaScript code: __closure__.

Before we're able to make sense of closure we need to make sure you have a _really_ good grasp on __scope__ in JavaScript.  Scope is the term that refers to where things like variables and functions can be used in your code.  

In the following example, do you know what will be logged on the last line?

```javascript
let a = 17;

const func = x => {
  let a = x;
};

func(99);

console.log(a); // ???????
```

Is it 17 or 99?  Do you know why?

The answer is 17, and the reason it's not 99 is that on line 4, the outer variable `a` is not redefined, rather a _new_ `a` is created inside the scope of that function.  In the end figuring out scope in most contexts is not all that complicated, but it is _crucial_ to understanding some of the more advanced concepts that are coming up soon, so take your time to understand what's going on in the following resources.

1. [This video](https://www.youtube.com/watch?v=SBwoFkRjZvE) is simple and clear!  Start here.

2. [This article](https://toddmotto.com/everything-you-wanted-to-know-about-javascript-scope/) starts simple and reiterates what the video covered, but goes deeper and is more specific about the appropriate terminology.  At the end he defines __closure__ _and_ describes the __module__ pattern, both of which we'll talk about more soon.

3. The previous article is great, but there is one inaccurate statement:

   > All scopes in JavaScript are created with `Function Scope` *only*, they aren’t created by `for` or `while` loops or expression statements like `if` or `switch`. New functions = new scope - that’s the rule

   that statement _was_ true in 2013 when the article was written, but ES6 has rendered it incorrect.  Read [this](http://wesbos.com/javascript-scoping/) article to get the scoop!

## Private Variables and Functions

Now that we've cemented your knowledge of scope in JavaScript take a look at this example:

```javascript
const FactoryFunction = string => {
  const capitalizeString = () => string.toUpperCase();
  const printString = () => console.log(`----${capitalizeString()}----`);
  return { printString };
};

const taco = FactoryFunction('taco');

printString(); // ERROR!!
capitalizeString(); // ERROR!!
taco.capitalizeString(); // ERROR!!
taco.printString(); // this prints "----TACO----"
```

I'm guessing you can figure this one out on your own, but I'll explain.  Because of the concept of scope neither of the functions created inside of `FactoryFunction` can be accessed outside of the function itself, which is why lines 9 and 10 above fail.  The only way to use either of those functions is to `return` them in the object (see line 4), which is why we can call `taco.printString()` but _not_ `taco.capitalizeString()`.  The big deal here is that even though _we_ can't access the `capitalizeString()` function, `printString()` can.  That is closure.

The concept of closure is the idea that functions retain their scope even if they are passed around and called outside of that scope.  In this case `printString` has access to everything inside of `FactoryFunction`, even if it gets called outside of that function.

Here's another example:

```javascript
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

In this example `counterCreator` initializes a local variable (`count`) and then returns a function.  To use that function we have to assign it to a variable (line 9).  Then, every time we run the function it `console.log`s `count` and increments it.  As above, the function `counter` is a closure.  It has access to the variable `count` and can both print and increment it, but there is no other way for our program to access that variable.

In the context of Factory Functions, closures allow us to create __private__ variables and functions.  Private functions are functions that are used in the workings of our objects that are not intended to be used elsewhere in our program. In other words, even though our objects might only do one or two things, we are free to split our functions up as much as we want (allowing for cleaner, easier to read code) and only export the functions that your program is actually going to use.

Using this terminology with our `printString` example from earlier, `capitalizeString` is a private function and `printString` is public. 

