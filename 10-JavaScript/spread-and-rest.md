# Rest Parameters & The Spread Operator

## Learning Goals

By the end of this lesson you should:

- Be able to use the rest parameter syntax to in functions which accept an unlimited number of arguments.
- Be able to explain what the spread operator does and where it is useful.

## Rest Parameters

Sometimes it's helpful to write a function that can take any number of arguments.  For example below, `sum` can only take two arguments

```javascript
const sum = function sum(x, y) {
  return x + y;
}

sum(3, 4); // 7
```

If we wanted to add up an unlimited number of arguments we could use a rest parameter by adding the `...` operator before the parameter name.

```javascript
const sum = function sum(...args) {
  // Loop through each item in args adding them together.  
  let sum = 0;
  args.forEach((item) => {
    sum += item;
  });
  return sum;
};

sum(3, 4, 5); // 12
sum(5); // 5
sum(3, 4); // 7

sum(); // 0
```

The parameter to the right of the `...` operator is **always** an array and contains any arguments unclaimed by parameters to the left of the rest parameter.  A rest parameter must be the last parameter in an argument list, but it does not have to be the only argument, for example.

```javascript
const greet = function greet(greeting, ...people) {
  console.log(`${greeting} ${people}`);
}

greet('Hello', 'Ada', 'Grace', 'Katherine'); // Hello Ada,Grace,Katherine
```

In the above example `greeting` takes the value `'hello'` and all the subsequent arguments are stored in the `people` array.  So rest parameters take the _rest_ of the arguments and put them into an array.  

A few things to highlight:

- A rest parameter must be the last item in a functional expression or declaration.
- A function may only have **one** rest parameter.

## The Spread Operator

So rest parameters let us take otherwise normal arguments and put them into an array.  Sometimes however you want to do the reverse.  It can be helpful to peel off elements of an array into arguments to a method.  

```javascript
const calculateBill = (appetizer, mainCourse, desert) => {
  return 1.08 * (appetizer + mainCourse + desert);
}

const prices = [3.75, 15.80, 10.0];

console.log (calculateBill(...prices)); // 31.914
```

In the above example:  `calculateBill(...prices)` is the equivalent to doing:  `calculateBill(prices[0], prices[1], prices[2]), a much more compact syntax.

The `Math.max` function is a good example of a function you might choose to use the spread operator on, it takes numeric arguments (unlimited size) and returns the largest item.  

```javascript
const list = [3, 7, 25, 1, -50];

const biggest = Math.max(...list);

console.log(biggest); // 25
```

**Wait so these look the same!**  While these both involve the same three periods, examine where the `...` operator is placed.  

- In rest parameters the `...` operator appears in the **function parameters** while in the example above, we used the spread operator to convert an array into individual items in the **function call**.  

- The spread operator can only be applied to [iterable values](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols) like arrays and strings while rest parameters take in any kind of arguments and place them into an array.

### Merging Arrays

You can also use the spread operator to expand an array to a list of elements for a new array.

```javascript
const newStudent = 'Ada';

const studentList = ['Charles', 'Grace', 'Katherine', 'Alexandra'];

const newList = [newStudent, ...studentList];

console.log(newList); // Ada,Charles,Grace,Katherine,Alexandra
```

**Challenge**  Use the spread operator to combine the two arrays below:

```javascript
const morningTracks = ['Master of Puppets', 'Black Metal', 'Paranoid'];
const eveningTracks = ['Ella Mai', 'American Teen', '17'];

// Merge the above arrays into a single array

```

<details>
  <summary>
    Click here to see an answer:
  </summary>

  `const combined = [...morningTracks, ...eveningTracks];`
</details>

### Spread operator with strings

You can also break a string into an array of individual characters with the spread operator.

```javascript
const letters = [...'abc']; // ['a', 'b', 'c']
```

### Spread operator with object literals

The spread operator can also be used to combine or merge two object literals.  The code below merges two object literals.

```javascript
const ada = {
  name: 'Ada Lovelace',
  age: 103,
};

const language = {
  name: 'C#',
  about: 'A strongly typed language by Microsoft',
};

const merged = { ...ada, ...language };

console.log(merged); // { name: 'C#', age: 103, about: 'A strongly typed language by Microsoft' }
```

Notice that it merges the objects in order, so that `ada`'s name is overwritten by that of `language`.  

## Examining Code

The following code simulates merging new permissions into a user account.  With a partner examine the code and answer the questions:

```javascript
const combinePreferences = function combinePreferences(user, ...preferences) {
  preferences.forEach((preference) => {
    user.preferences = { ...user.preferences, ...preference}
  });
  return user;
}

const user = {
  name: 'ada',
  preferences: {
    superuser: true,
    billableUser: false,
  }
};

const billing = {
  billableUser: true,
  getsReports: true,
  canDeployApps: false,
};

const project = {
  canCreateProjects: false,
  canDeployApps: false,
  canDeployToTest: true,
};

console.log(combinePreferences(user, billing, project));
```

**Questions** 

As a pair read the code and predict the result.  Then run the resulting code in the terminal.

1. Identify where the spread and rest parameter operators are.
2. Did it output what you expected?  Why?
3. Is the `forEach` loop necessary?  Could it be done without a loop?  Why or why not?

<details>
  <summary>
    Click here to see some answers.
  </summary>

  1. The rest parameter occurs at `combinePreferences(user, ...preferences)`, and the spread operator is used twice at `user.preferences = { ...user.preferences, ...preference}`.  
  2. The result is the user object with it's preference attribute updated merging them in order of appearance.
  3. The `forEach` loop is needed because using the spread operator on an array will result in merging in the array's keys (index numbers) and values into the preferences.  

</details>


## Review

- Rest parameters can be used to write a method taking an unlimited number of arguments.  
- Using rest parameters arguments are added to an array parameter.
- The spread operator does the inverse, taking an iterable object, like an array or string and converting it to individual items to be used in a function call, or instantiating a method.
- The spread operator can also be used to merge objects.

## Resources

- [MDN on Rest Parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)
- [MDN on Spread Operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
- [JavaScript.info on Rest & Spread](https://javascript.info/rest-parameters-spread-operator)