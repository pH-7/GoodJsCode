# The GoodJsCode™ Guidebook

## Write elegant code by following the good practices 🚀

I'm [Pierre-Henry Soria](https://ph7.me). An enthusiastic and passionate software engineer. Originally from Brussels, (Belgium), I'm currently living in the wonderful land called “Australia” (Adelaide).

I've been coding for over 10 years. Today, I decided to share my knowledge in terms of writing good code.

I review daily hundreds of lines of code. Brand new micro-services, new features, new refactoring, hotfixes, and so on. I've seen so many different coding styles as well as good and bad coding habits from the developers I've been working with.

With this book, you will get **the essential to know**, straight to the solution for writing better and cleaner code. It's a practical book. You won't have superfluous information. Just the important things ✅

Time is so valuable and important (even more as a software engineer), so I will only share what you need to know, without unnecessary details, to be there only for making the book fatter.


---

## 📖 Table of Contents

- [The “One Thing” principle](#the-one-thing-principle-1%EF%B8%8F⃣)
- [Don’t comment on what it does. Write what it does.](#dont-comment-on-what-it-does--write-what-it-does-)
- [Boat anchor - Unused code](#boat-anchor---unused-code)
- [Minimalist code](#minimalist-code)
- [You Aren't Going To Need This... (a.k.a. YAGNI)](#you-arent-gonna-need-it-yagni)
- [Reuse your code across your different projects by packing them into small NPM libraries](#reuse-your-code-across-your-different-projects-by-packing-them-into-small-npm-libraries)
- [Tests come first. Never Last](#-tests-come-first-never-last)
- [Import only what you need](#import-only-what-you-need)
- [Conditions into clear function names](#conditions-into-clear-function-names)
- [Readable Name: Variables](#readable-name-variables)
- [Readable Name: Functions](#readable-name-functions)
- [Readable Name: Classes](#readable-name-classes)
- [Guard Clauses approach](#guard-clauses-approach)
- [.gitignore and .gitattributes to every project](#gitignore-and-gitattributes-to-every-project)
- [Demeter Law](#demeter-law)
- [Debugging efficiently](#debugging-efficiently)
- [Fewer arguments is more efficient](#fewer-arguments-is-more-efficient)
- [Stub/mock only what you need](#stubmock-only-what-you-need)
- [Remove the redundant things](#remove-the-redundant-things)
- [Ego is your enemy](#ego-is-your-enemy-)
- [Don’t use abbreviations](#dont-use-abbreviations-)
- [American English spelling. The default choice when coding](#-american-english-spelling-the-default-choice-when-coding)
- [Destruct array elements in a readable way](#destructing-array-elements---make-it-readable)
- [Readable numbers](#readable-numbers)
- [Avoid “else-statement”](#avoid-else-statement)
- [Prioritize `async`/`await` over Promises](#prioritize-asyncawait-over-promises)
- [No magic numbers](#prioritize-asyncawait-over-promises)
- [Always use `assert.strictEqual`](#always-use-assertstrictequal)
- [Updating an object - The right way](#updating-an-object---the-right-way)
- [Stop using `Date()` when doing benchmarks](#stop-using-date-when-doing-benchmarks)
- [Lock down your object](#lock-down-your-object-)
- [Consider aliases when destructing an object](#consider-aliases-when-destructing-an-object)
- [Always use the strict type comparison](#always-use-the-strict-type-comparison)
- [Avoid using `default export` as much as you can](#avoid-using-default-export-as-much-as-you-can)
- [Always write pure functions](#always-write-pure-functions)
- [Linters and Formatters](#linters-and-formatters)
- [About the author](#about-the-author)


## The “One Thing” principle 1️⃣

When writing a function, remind yourself that it should ideally do only one (simple) thing. Think about what you have already learned concerning the comments. The code should say everything. No comments should be needed. Splitting the code into small readable functions and reusable portions of code will drastically make your code readable and eliminate the need to copy/paste the same piece of code just because it hasn’t been properly moved into reusable functions/classes.

### ❌ Non-readable function

```javascript
function retrieveName(user) {
  if (user.name && user.name !=== 'admin' && user.name.length >= 5) {
    return user.name;
  }
  // ...
}
```

### ✅ One Thing. Neat & Clean

```javascript
const isRegularUser = (name) => {
  return name !=== config.ADMIN_USERNAME;
}

const isValidNameLength = (name, minimumLength = 5) => {
  return name.length >= minimumLength;
}

const isEligibleName(name) {
  return isRegularUser(name) && isValidNameLength(name);
}

// …

function retrieveName(user) {
  const name = user?.name;

  if (isEligibleName(name)) {
    return user.name;
  }
}
```


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Don’t comment on what it does ❌ Write what it does ✅

It's crucial to name your functions and variables in simple and explicit words so that they say what they do (just by reading their names).

If the code requires too many comments to be understood, the code needs to be refactored. The code has to be understood by reading it. Not by reading the comments. And the same applies when you write tests.

Your code has to be your comments. At the end of the day, as developers, we tend to be lazy and we don’t read the comment (carefully). However, the code, we always do.

### ❌ Bad practice

```javascript
let validName = false;
// We check if name is valid by checking its length
if (name.length >= 3 && name.length <= 20) {
  validName = true;
  // Now we know the name is valid
  // …
}
```

### ✅ Good example

```javascript
const isValidName = (name) => {
  return (
    name.length >= config.name.minimum && name.length <= config.name.maximum
  );
};
// …

if (isValidName('Peter')) {
  // Valid ✅
}
```


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Boat anchor (AKA [Tree Shaking](https://developer.mozilla.org/en-US/docs/Glossary/Tree_shaking))

Never keep some unused code or commented code, _just in case_ for history reason.
Sadly, it’s still very common to find commented code in PRs.

Nowadays, everybody uses a version control system like git, so there is always a history where you can look and go backwards if needed.

### ❌ Downside of keeping unused code

1. We think we will come back removing it when it’s time. Very likely, we will et busy on something else and we will forget removing it.
2. The unused code will add more complexity for a later refactoring.
3. Even if unused, it will still show up when searching in our codebase (which adds more complexity too).
4. For new developers joining the project, they don’t know if they can remove it or not.

### ✅ Action to take

Add a BitBucket/GitHub pipeline or a git hook on your project level for rejecting any unused and commented code.


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Minimalist code

Coding in a minimalist way is the best pattern you can follow! Simplicity over complexity always wins! 🏆

Each time you need to create a new feature or add something to a project, see how you can reduce the amount of code.

There are so many ways to achieve a solution. And there is always a shorten and cleaner version which should always be the chosen one.

Think twice before starting writing your code. Ask yourself “*what would be the simplest and most elegant solution I can write*”, so that the written code can be well-maintained over time and very easily understood by other developers who don't have the context/acceptance criteria in mind.

Brainstorm about it. Later, you will save much more time while writing your code.


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->


## You Aren't Gonna Need It (YAGNI)

❌ Don't code things “*just in case*” you might need it for later.

Don't spend time and resources on what you might not need.


✅ You need to solve today's problem today and tomorrow's problem tomorrow.



**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->


## Reuse your code across your different projects by packing them into small NPM libraries

### ❌ Wrong approach

My project is small (well, it always starts small).
I don’t want to spend time splitting functionalities into separated packages. Later on, somehow, my project grow bigger and bigger indeed. However, since I haven’t spent time splitting my code into packages at the beginning. Now, I think it will take even longer to refactor my code into reusable packages.
In short, I’m in a trap. My architecture is just not scalable.

### ✅ Right approach

Always think about reusibility. No matter how small or big is your project.

Splitting your code into small reusable external packages will always speed you up in the long run. For instance, there will be times where you will need a very similar functionality to be used in another project for another client’s application.

Whether you are building a new project from scratch or implementing new features into it, always think about splitting your code early into small reusable internal NPM packages, so other potential products will be able to benefit from them and won’t have to reinvent the wheel.
Moreover, your code will gain in consistency thanks to reusing the same packages.

Finally, if there is an improvement or bug fix needed, you will have to change only one single place (the package) and not every impacted project.

Icing on the cake, you can make public some of your packages by open source them on GitHub and other online code repositories to get some free marketing coverage and potentially some good users of your library and contributors as well 💪



**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## 🏁 Tests come first. Never last

**Never wait until the last moment to add some unit tests of the recent changes you have added**. 

Too many developers underestimate their tests during the development stage.

Don’t create a pull request without unit tests. The other developers reviewing your pull request are not only reviewing your changes but also your tests. 

Moreover, without unit tests, you have no idea if you introduce a new bug. Your new changes may not work as expected. 
Lastly, there will be chances you won’t get time or rush up writing your tests if you push them for later.

### ❌ Stop doing

Stop neglecting the importance of unit tests. Tests are there to help you developing faster in the long run.

### ✅ Start doing

Create your tests even before changing your code. Create or update the current tests to expect the new behavior you will introduce in the codebase. Your test will then fail. Then, update the `src`  of the project with the desired changes.
Finally, run your unit tests. If the changes are correct and your tests are correctly written, your test should now pass 👍 Well done! You are now following the TDD development approach 💪

Remember, unit tests are there to save your day 🎉 


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Import only what you need

Have the good practice of importing only the functions/variables you need. This will prevent against function/variable conflicts, but also optimizing your code/improving readability by only expose the needed functions.

### ❌ Importing everything

```javascript
import _ from 'lodash';

// …

if (_.isEmpty(something)) {
  _.upperFirst(something);
}
```

### ✅ Import only the needed ones

```javascript
import { isEmpty as _isEmpty, upperFirst as _upperFirst } from 'lodash';

// …

if (_isEmpty(something)) {
  _upperFirst(something);
}
```


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Conditions into clear function names

### ❌ Non-readable condition

```javascript
const { active, feature, setting } = qrCodeData;

if (active && feature === 'visitor' && setting.name.length > 0) {
  // …
}
```

The condition isn't easy to read, long, not reusable, and would very likely need to be documented as well (making the whole coding experience tedious).

### ✅ Clear boolean function

```javascript
const canQrCode = ({ active, feature, setting }, featureName) => {
  return active && feature === 'visitor' && setting.name.length > 0;
};

// …

if (canQrCode(qrCodeData, 'visitor')) {
  // …
}
```

Here, the code doesn’t need to be commented. The boolean conditional function says exactly what it does, producing a readable and clean code 🚀

🍰 Cherry on the cake, the code is scalable. Indeed, the function `canQrCode` can be placed in a service or helper, increasing the reusability and decreasing the chance of duplicating code.


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Readable Name: Variables

Mentioning clear good and explicit names for your variables is critical to prevent confusion. Sometimes, we spend more time understanding what a variable is supposed to contain rather than achieving the given task.

### ❌ Bad Variable Name

```javascript
// `nbPages` naming doesn’t mean much ❌
const nbPages = postService.getAllItems();

let res = '';
for (let i = 1; i <= nbPages; i++) {
  res += '<a href="?page=' + i + '">' + i + '</a>';
  }
}
```

Giving `i` for the name of the increment variable is a terrible idea. Although, this is the standard for showing examples with the `for` loop, you should never do the same with your production application (sadly, plenty of developers just repeat what they've learned. Of course, we can’t blame them, but it's now time to change!).

Every time you declare a new variable, look at the best and short words you can use to describe what it stores.

### ✅ Good example, with a clear variable name

```javascript
// `totalItems` is a much better name ✅
const totalItems = postService.getAllItems();
// …

let htmlPaginationLink = '';
for (let currentPage = 1; currentPage <= totalItems; currentPage++) {
  htmlPaginationLink +=
    '<a href="?page=' + currentPage + '">' + currentPage + '</a>';
}
```


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Readable Name: Functions

### ❌ Complicated (vague/unclear) function name

```javascript
function cleaning(url) {
  // …
}
```

### ✅ Explicit descriptive name

```javascript
function removeSpecialCharactersUrl(url) {
  // …
}
```

Each word of a function name should be capitalized except the first letter of the function. This is know as **lowerCamel** case, like `isNameValid()`.

**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Readable Name: Classes

### ❌ Generic/Vague name

```javascript
class Helper { // 'Helper' doesn't mean anything

  stripUrl(url) {
    // ...
    return url.replace('&amp;', '');
  }

  // ...
}
```

The class name is vague and doesn’t clearly communicate what it does. By reading the name, we don’t have a clear idea of what `Helper` contains and how to use it.

### ✅ Clear/Explicit name

```javascript
class Sanitizer { // <- Name is already more explicit

  constructor(value) {
    this.value = value;
  }

  url() {
    this.value.replace('&amp;', '');
    // ...
  }
}
```

In this case, the class name clearly says what it does. It’s the opposite of a black box. By saying what it does, it should also follow the single responsibility principle of achieving only one single purpose.
Class names should be a (singular) noun that starts with a capital letter. The class can also contain more than one noun. If so, each word has to be capitalized (this is called **UpperCamel** case).


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Guard Clauses approach

The guard clauses pattern is the way of leaving a function earlier by removing the redundant `else {}` blocks after a `return` statement.

Let’s see a snippet that doesn’t follow the guard clause pattern and a clean and readable example that does.

The two samples represent the body of a function. Inside of the function we have the following 👇

### ❌ The “not-readable” way

```javascript
if (payment.bonus) {
  // … some logics
  if (payment.bonus.voucher) {
    return true; // eligible to a discount
  } else if (payment.confirmed) {
    if (payment.bonus.referral === 'friend') {
      return true;
    } else {
      return false;
    }
  }
  return false;
}
```

### ✅ Clean readable way

```javascript
if (!payment.bonus) {
  return false;
}

if (payment.bonus.voucher) {
  return true;
}

if (payment.bonus.referral === 'friend') {
  return true;
}

return false;
```

On this example, we can notice how we could remove the complicated nested conditionals thanks to exiting the function as early as possible with the `return` statement.


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## .gitignore and .gitattributes to every project

When you are about to distribute your library, you should always create a `.gitignore` and `.gitattributes` in your project to prevent undesirable files to be presented in there.

### ✅ `.gitignore`

As soon as you commit files, your project needs a `.gitignore` file. It guarantees to exclude specific files from being committed in your codebase.

### ✅ `.gitattributes`

When you publish your package to be used by a dependency manager, it’s crucial to exclude the developing files (such as the `tests` folders, `.github` configuration folder, `CONTRIBUTING.md`, `SECURITY.me`, `.gitignore`, `.gitattributes` itself, and so on…).
Indeed, when you install a new package through your favorite package manager (npm, yarn, …), **you only want to download the required source files, that’s all** without including the test files, pipeline configuration files, etc, not needed for production.


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Demeter Law

The **demeter law**, AKA the **principle of least knowledge** states that each unit of code should only have very limited knowledge about other units of code. They should only talk to their close friends, not to their strangers 🙃 It shouldn’t have any knowledge on the inner details of the objects it manipulates.

### ❌ Chaining methods

```javascript
object.doA()?.doB()?.doC(); // violated deleter’s law
```

Here, `doB` and `doC` might receive side-effects from their sub-chaining functions.

### ✅ Non-chaining version

```javascript
object.doA();
object.doB();
object.doC();
```

Each method doesn’t rely on each other. They are independent and safe from eventual refactoring.


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Debugging efficiently

When debugging with arrays or objects, it’s always a good practice to use `console.table()`

### ❌ `console.log(array)`

`console.log` makes the result harder to read. You should always aim for the most efficient option.

```javascript
const favoriteFruits = ['apple', 'bananas', 'pears'];
console.log(favoriteFruits);
```

![Debugging with simple console.log](https://user-images.githubusercontent.com/1325411/158784062-92c770d5-1887-413d-91ad-58755c57def4.png)

### ✅ `console.table(array)`

Using `console.table` saves you time as the result is shown in a clear table, improving readability of the log when debugging an array or object.

Mozilla gives us a clear example to see how `console.table` can help you.

```javascript
const favoriteFruits = ['apple', 'bananas', 'pears'];
console.table(favoriteFruits);
```

![With console.table](https://user-images.githubusercontent.com/1325411/158784071-136971a3-8a81-448c-8c88-0c866cbcc6a8.jpeg)


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Fewer arguments is more efficient

If your functions have more than 3 arguments, it means you could have written a much better and cleaner code. In short, the purpose of your function does too much and violates the single responsibility principle, leading to debugging and reusability hells.

In short, the fewer arguments your function has, the more efficient it will become as you will prevent complexity in your code.

### ❌ Unclean code

```javascript
function readItem(
  id: number,
  userModel: UserModel,
  siteInfoModel: SiteModel,
  security: SecurityCheck
) {}
```

### ✅ Clean code

```javascript
user = new User(id);
// …
function readItem(user: User) {}
```

With this version, because it has only relevant arguments, reusing the function elsewhere will be possible as the function doesn’t rely or depend on unnecessary arguments/objects.


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Stub/mock only what you need

When you stub an object in your tests (with Sinon for instance), it’s a good and clean practice to decide only what functions you need to stub out, instead of stubbing out the entier object. Doing so, you also prevent overriding internal implementations of functions which cause all sorts of inconsistencies in your business logic.

### ❌ Everything is stubbed

```javascript
sinon.stub(myObject);
```

### ✅ Only the function you need is stubbed

```javascript
sinon.stub(myObject, 'myNeededFunction');
```

Here, we only stub out the individual function we need.


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Remove the redundant things

When we code, we often tend to write "unnecessary" things, which don't increase the readability of the code either.

For instance, in a switch-statement, having a `default` clause that isn’t used.

### ❌ Redundant version

```javascript
const PRINT_ACTION = 'print';
const RETURN_ACTION = 'return';
// …
switch (action) {
  case PRINT_ACTION:
    console.log(message);
    break;

  case RETURN_ACTION:
    return message;
    break; // ❌ This is redundant as we already exit the `switch` with `return`

  default: // ❌ This clause is redundant
    break;
}
```

### ✅ Neat version

```javascript
const PRINT_ACTION = 'print';
const RETURN_ACTION = 'return';
// …
switch (action) {
  case PRINT_ACTION:
    console.log(message);
    break;
  case RETURN_ACTION:
    return message;

  default:
    throw new Error(`Invalid ${action}`);
}
```

Here, we keep the `default` clause, but we take benefit of it by throwing an exception.


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Ego is your enemy ✋

Too often I see developers taking the comments on their pull requests personally because they see what they have done as their own creation. When you receive a change request, don’t feel judged! This is actually an improvement reward for yourself 🏆

If you want to be a good developer, leave your ego in your closet. Never bring it to work. This will just slow your progression down and could even pollute your brain space and the company culture.

### ❌ Taking what others say as personally

### ✅ See every feedback as a learning experience

> When you write code, it’s not your code, it’s everybody’s else code. Don’t take what you write personally. It’s just a little part of the whole vessel.


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Don’t use abbreviations 😐

Abbreviations are hard to understand. For new joiners who aren’t familiar with the company’s terms. And even with common English abbreviations, they can be quite difficult to be understood by non-native English speakers who might be hired in the future or when outsourcing developers from overseas.

Having as a golden rule to never use abbreviations in your codebase (so at least, you’ll never have to take any further decisions on this topic) is critical for preventing confusion later on.

### ❌ Difficult to read. Hard to remember over time

```javascript
if (type === Type.PPL_CTRL) {
  // We are on People Controller
  // Logic comes here
}

if (type === Type.PPL_ACT) {
  // We are on People Action
  // …
}

if (type === Type.MSG_DMN_EVT) {
  // We are on Messaging Domain Event
  // …
}

// ...

const IndexFn = () => {
  // index function
}
```

### ✅ Clear names (without comments needed 👌)

```javascript
if (type === Type.PEOPLE_CONTROLLER) {
  // Logic comes here
}

if (type === Type.PEOPLE_ACTION) {
  // Logic here
}

if (type === Type.MESSAGING_DOMAIN) {
  // Logic here
}

// ...

const index = () => {
  // No need to have `Fn` or ˋFunction` as suffix in the name
  // Having ˋfunction` or ˋmethod` for an actual function is redundant and generally bad practice
  // ...
}
```


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## 🇺🇸 American English spelling. The default choice when coding

I always recommend to use only **US English** in your code. If you mix both British and American spellings, it will introduce some sort of confusion for later and might lead to interrogations for new developers joining the development of your software.

Most of the 3rd-party libraries and JavaScript's reserved words are written in American English. As we use them in our codebase, we should prioritize US English as well in our code.
I’ve seen codebase with words such as “_licence_” and “_license_”, “_colour_” and “_color_”, or “_organisation_” and “_organization_”.
When you need to search for terms / refactor some code, and you find both spellings, it requires more time and consumes further brain space, which could have been avoided in the first place by following a consistent spellings.

Finally, I’ve noticed that it's easier to mispell words with the British spelling like typing “colur” instead of “colour”.


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Destructing array elements - Make it readable

When you need to destruct an array with JavaScript (ES6 and newer), and you want to pickup only the second or third array, there is a much cleaner way than using the `,` to skip the previous array keys.

### ❌ Bad Way

```javascript
const meals = [
  'Breakfast',
  'Lunch',
  'Apéro',
  'Dinner'
];

const [, , , favoriteMeal] = meals;
console.log(favoriteMeal); // Dinner
```

### ✅ Recommended readable way

```javascript
const meals = [
  'Breakfast', // index 0
  'Lunch', // index 1
  'Apéro', // index 2
  'Dinner' // index 3
];

const { 3: favoriteMeal } = meals; // Get the 4th value, index '3'
console.log(favoriteMeal); // Dinner
```

Here, we destruct the array as an object with its index number and give an alias name `favoriteMeal` to it.


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Readable numbers

With JavaScript, you can use **numeric separators** and **exponential notations** to make numbers easier to read. The execution of the code remains exactly the same, but it’s definitely easier to read.

### ❌ Without numeric separators

```javascript
const largeNumbers = 1000000000;
```

### ✅ Clear/readable numbers

```javascript
const largeNumbers = 1_000_000_000;
```

_Note: numeric separators works also with other languages than JavaScript such as Python, Kotlin, …_


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Avoid “else-statement”

Similar to what I mentioned concerning the importance of writing portions of code that only do “**One Thing**”, you should also avoid using the _if-else statement_.

Too many times, we see code such as below with unnecessary and pointless `else` blocks.

### ❌ Conditions with unnecessary `else {}`

```javascript
const getUrl = () => {
  if (options.includes('url')) {
    return options.url;
  } else {
    return DEFAULT_URL;
  }
}
```

This code could easily be replaced by a clearer version.

### ✅ Option 1. Use default values

```javascript
const getUrl = () => {
  let url = DEFAULT_URL;

  if (options.includes('url')) {
    url = options.url;
  }
  return url;
}
```

By having a default value declared in an upper variable, we remove the need of a `else {}` block.

### ✅ Option 2. Use Guard Clauses approach

```javascript
const getUrl = () => {
  if (options.includes('url')) {
    return options.url; // if true, we return `options.url` and leave the function
  }
  return DEFAULT_URL;
}
```

With this approach, we leave the function early, preventing complicated and unreadable nested conditions in the future.


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Prioritize `async/await` over Promises

### ❌ With promises

```javascript
const isProfileNameAllowed = (id) => {
  return profileModel.get(id).then((profile) => {
    return Ban.name(profile.name);
  }).then((ban) => ({
    return ban.value
  }).catch((err) => {
    logger.error({ message: err.message });
  });
}
```

Promises make your code harder to read and tend to increase code [cyclomatic complexity](https://en.wikipedia.org/wiki/Cyclomatic_complexity).

### ✅ With `async`/`await`

```javascript
const isProfileNameAllowed = async (id) => {
  try {
    const profile await profileModel.get(id);
    const {value: result } = await Ban.name(profile.name);
    return result;
  } catch (err) {
    logger.error({ message: err.message });
  }
}
```

By using `async`/`await`, you avoid callback hell, which happens with promises chaining when data are passed through a set of functions and leads to unmanageable code due to its nested fallbacks.


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## No magic numbers 🔢

Avoid as much as you can to hardcode changeable values which could change over time, such as the number of total posts to display, timeout delay, and other similar information.

### ❌ Code containing magic numbers

```javascript
setTimeout(() => {
  window, (location.href = '/');
}, 3000);
```

```javascript
getLatestBlogPost(0, 20);
```

### ✅ No hardcoded numbers

```javascript
setTimeout(() => {
  window, (location.href = '/');
}, config.REFRESH_DELAY_IN_MS);
```

```javascript
const POSTS_PER_PAGE = 20;

getLatestBlogPost(0, POSTS_PER_PAGE);
```


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Always use `assert.strictEqual`

With your test assertion library (e.g. chai), always consider using the strict equal assertion method.

### ❌ Don’t just use `assert.equal`

```javascript
assert.equal('+63464632781', phoneNumber);
assert.equal(validNumber, true);
```

### ✅ Use appropriate strict functions from your assertion library

```javascript
assert.strictEqual('+63464632781', phoneNumber);
assert.isTrue(validNumber);
```


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Updating an object - The right way

### ❌ Avoid `Object.assign` as it’s verbose and longer to read

```javascript
const user = Object.assign(data, {
  name: 'foo',
  email: 'foo@bar.co',
  company: 'foo bar inc',
});
```

### ✅ Use destructing assignment, spread syntax

```javascript
const user = {
  ...data,
  name: 'foo',
  email: 'foo@bar.co',
  company: 'foo bar inc',
};
```


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Stop using `Date()` when doing benchmarks

JavaScript is offering a much nicer alternative when you need to measure the performance of a page during a benchmark.

### ❌ Stop doing

```javascript
const start = new Date();
// your code ...
const end = new Date();

const executionTime = start.getTime() - end.getTime();
```

### ✅ With ` performance.now()

```javascript
const start = performance.now();
// your code ...
const end = performade.now();

const executionTime = start - end;
```


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Lock down your object 🔐

It’s always a good idea to const lock the properties when creating an object. That way, the values of your properties object will only be read-only.

### ❌ Without locking an object

```javascript
const canBeChanged = { name: 'Pierre' }:

canBeChanged.name = 'Henry'; // `name` is now "Henry"
```

### ✅ With `as const` to lock an object

```javascript
const cannotBeChanged = { name: 'Pierre' } as const;

cannotBeChanged = 'Henry'; // Won't be possible. JS will throw an error as `name` is now readonly
```


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Consider aliases when destructing an object

### ❌ Without aliases

```javascript
const { data } = getUser(profileId);
const profileName = data.name;
// ...
```

### ✅ With clear alias name

```javascript
const { data: profile } = getUser(profileId);
// then, use `profile` as the new var name 🙂
const profileName = profile.name;
// ...
```


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Always use the strict type comparison

When doing some kind of comparison, always use the `===` strict comparison.

### ❌ Don’t use loose comparisons

```javascript
if ('abc' == true) {
} // this gives true ❌

if (props.address != details.address) {
} // result might not be what you expect
```

### ✅ Use strict comparisons

```javascript
if ('abc' === true) {
} // This is false ✅

if (props.address !== details.address) {
} // Correct expectation
```


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->


## Avoid using export default as much as you can

The main reason why you should avoid `export default` is that is makes refactoring very complex when you need to rename a class or a component name.

You will have to update every name of your imports as it will need to match with the actual name of your default export class/function/component.

When working on a large-scale project, in addition to spending more time, you will also increase the chance of forgetting renaming an import (or simply having a typo).

Import each function/class seperately will helps your IDE's IntelliSense to picking up and auto-import correctly when refactoring, which won't be the case with a renamed `default export`.

At the end of the day though, being consistent with your project and your team on what coding flavour and convention you choose is also to take into consideration.


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->


## Always write pure functions

> If a tree falls in the woods, does it make a sound?
> If a pure function mutates some local data in order to produce an immutable return value, is this okay?
> Rich Hickey. Creator of the [Clojure](https://en.wikipedia.org/wiki/Clojure)


Given a specific input (argument) to a function, a pure function <ins>always returns the same output</ins> as the pure function doesn't modify their input values.

A pure function will never produce side effects, meaning that it doesn't change any external states from another function. The pure function only depends on its input arguments and on the function's scope itself. With them, you can focus your attention in only one place, which makes a huge difference when reading and debugging your code.

```javascript
function addition(x, y) {
    return x + y;
}
```

A trickier scenario can occur when you are passing an object.
Imagine you are passing a “user” object to another function. If you modify the object “user” in the function, it will modify the actual user object because the object passed in as a parameter is actually a reference of the object, which is the opposite of a distinct new cloned object. 

To prevent this downside, you will have to deep-clone the object first (you can use the Lodash *cloneDeep* function) and then `Object.freeze(copyUser)` when returning it. This will guarantee the “copyUser” to be immutabled.

For instance:

```javascript
import { cloneDeep as _cloneDeep } from 'lodash';

function changeUser(user) {
  // copyUser = { …user };
  const copyUser = _cloneDeep(user);
  copyUser.name = 'Edward Ford';
  
  return Object.freeze(copyUser);
}
```

✅ By writing pure functions, you will make the code easier to read and understand. You only need to focus your attention on the function itself without having to look at the surrounding environments, states, and properties outside the function's scope, preventing you to spend hours debugging.


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->


## Use the TODO and FIXME prefix in your comments (when really need to comment)

If you have to come back and change something later, you might need to comment in your code, but then you must use the TODO or FIXME prefix, so that your code will be highlighted in the majority of IDEs.

Finally, I would also suggest you the VS Code extension [TODO Tree](https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.todo-tree) that cretes a nice todo list.

✅ Example 👇

```javascript
// TODO <JIRA-TICKET-ID> Change the logic to reflect to the release of productB
function somethingMeaningful() {}
```


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->


## Linters and Formatters

Indentation is also very important. Having consistent code that follows the same coding conventions across your products will help to ship clean and readable code.

For doing so, it’s crucial to use ESLint and Prettier on your code editor (e.g., VS Code) as well as set up some git hooks (on pre-commit or pre-push hook) which will run ESLint as a pre-check for when you git commit/push your code.

Finally, you can very easily set up a GitHub workflow action for your project.


**[⬆️ Back to top](#-table-of-contents)**

---

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->


## About the Author

**[Pierre-Henry Soria](https://ph7.me)**. A super passionate and enthusiastic software engineer, and a true cheese & chocolate lover 💫 

☕️ Did you enjoy this guideline book (that will most likely take your developer career to the next level)? If so, would you consider **[offering me a coffee](https://ko-fi.com/phenry)**? 😊

[![@phenrysay](https://img.shields.io/badge/Twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white)](https://twitter.com/phenrysay) 
 [![pH-7](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/pH-7)

[![Pierre-Henry Soria](https://s.gravatar.com/avatar/a210fe61253c43c869d71eaed0e90149?s=200)](https://ph7.me "Pierre-Henry Soria personal website")
