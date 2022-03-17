# Coode JS Code


## 📖 Table of Contents


1. [The “One Thing” principle 1️⃣](#the-one-thing-principle-1%EF%B8%8F⃣)
2. [Don’t comment what it does. Write what it does.](#dont-comment-what-it-does--write-what-it-does-)
3. [Conditions into clear function names](#conditions-into-clear-function-names)
4. [Boat anchor - Unused code](#boat-anchor---unused-code)
5. [Minimalist code](#minimalist-code)
6. [Reuse your code across your different projects by packing them into small NPM libraries](#reuse-your-code-across-your-different-projects-by-packing-them-into-small-npm-libraries)
7. [Tests come first. Never Last](#tests-come-first--never-last-)
8. [Readable Name: Variables](#readable-name-variables)
9. [Import only what you need](#import-only-what-you-need)
10. [Readable Names: Functions](#readable-names-functions)
11. [Guard Clauses approach](#guard-clauses-approach)


## The “One Thing” principle 1️⃣

When writing a function, remind yourself that it should ideally do only one thing. Think about what you learned already concerning the comments. The code should say everything. No comments should be needed. Splitting the code into small readable functions and reusable portions of code will drastically make your code readable and eliminate the need of copy/paste the same piece of code just because they haven’t been properly moved into reusable functions/classes.

### ❌ Non-readable function

```javascript
function retrieveName(user) {
  if (user.name && user.name !=== 'admin' && user.name.length >= 5) {
    return user.name;
  }
}
```

### ✅ One Thing. Read & Clear

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

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Don’t comment what it does ❌ Write what it does ✅

It’s crucial to name your functions and variable names exactly what they do.

If the code requires too many comments to be understood, the code needs to be refactored. The code has to be understood by reading it. Not by reading the comments. And the same applies when you write tests.

Your code has to be your comments. At the end of the day, as developers, we tend to be lazy and we don’t read the comment (carefully). However, the code, we always do.

### ❌ Bad practice

```javascript
let validName = false;
// We check if the name of valid by checking its length
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

The condition is hard to read, long, not reusable, and would very likely need to be documented as well.

### ✅ Clear boolean conditional function

```javascript
const canQrCode = ({ active, feature, setting }, featureName) => {
  return active && feature === 'visitor' && setting.name.length > 0;
};

// …

if (canQrCode(qrCodeData, 'visitor')) {
  // …
}
```

Here, the code doesn’t need to be commented. The boolean function says what it does, producing a readable and clean code.
Icing on the cake, the code is scalable. Indeed, the function `canQrCode` can be placed in a service or helper, increasing the reusability and decreasing code duplications.

**[⬆️ Back to top](#-table-of-contents)**

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Boat anchor - Unused code

Never keep some unused code or commented code, _just in case_ for history reason.
Sadly, it’s still very common to find commented code in PRs.

Nowadays, everybody uses a version control system like git, so there is always a history where you can go backwards.

### ❌ Downside of keeping unused code

1. We think we will come back removing it when it’s time. Very likely, we will et busy on something else and we will forget removing it.
2. The unused code will add more complexity for a later refactoring.
3. Even if unused, it will still show up when searching in our codebase (which adds more complexity too).
4. For new developers joining the project, they don’t know if they can remove it or not.

### ✅ Action to take

Add a BitBucket/GitHub pipeline or a git hook on your project level for rejecting any unused/commented code.

**[⬆️ Back to top](#-table-of-contents)**

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Minimalist code

Coding in a minimalist way is the best pattern you can follow.

Each time you need to create a new feature or add something to a project, see how you can reduce the amount of code. There are so many ways to achieve the solution. And there is always a shorten and clearer version which should always be the chosen one. Think twice before starting writing your code what would be the simplest solution you can write.
Brain storm about it. Later, you will save much more time while writing your code.

**[⬆️ Back to top](#-table-of-contents)**

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Reuse your code across your different projects by packing them into small NPM libraries

### ❌ Wrong approach

My project is small (well, it always starts small).
I don’t want to spend time splitting functionalities into separated packages. Later on, somehow, my project grow bigger and bigger indeed. However, since I haven’t spent time splitting my code into packages at the beginning. Now, I think it will take even longer to refactor my code into reusable packages.
In short, I’m in a trap. My architecture is just not scalable.

### ✅ Right approach

Always think about reusibility. No matter how small or big is your project.
Splitting your code into small reusable external packages if the golden path. Very likely, you will need a very similar functionality to another project for a another client’s application.

Whether you are building a new project from scratch or implementing new features into it, always think about splitting your code early into small reusable internal NPM packages, so other potential products will be able to benefit from them and won’t have to reinvent the wheel.
Moreover, your code will gain in consistency thanks to reusing the same packages.

Finally, if there is an improvement or bug fix needed, you will have to change only one single place (the package) and not every impacted project.

🍰 Icing on the cake, you can make public some of your packages by open source them on GitHub and other online code repositories to get some free marketing coverage and potentially some good users of your library and contributors as well 💪


**[⬆️ Back to top](#-table-of-contents)**

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Tests come first 🏁 Never last ❌

Never wait for the last moment to add some unit tests of the recent changes you have added. Too many developers underestimate their tests during the development stage.

Don’t create a pull request without unit tests. The other developers reviewing your pull request are not only reviewing your changes but also your tests. Moreover, without unit tests, you have no idea if you introduce a new bug. Your new changes may not work as expected.
Lastly, there will be much chance you won’t get time or rush up writing your tests if you push to later writing tests.

### ❌ Stop doing

Never underestimate the importance of unit tests. Tests are there to help you developing fast in the long run.

### ✅ Start doing

Create your tests even before changes your code. Create or update the current tests to expect the new behavior your will introduce in the codebase. Your test will then fail. Then, update the `src` of the project with the desire changes.
Finally, run your unit tests. If the changes are correct and your tests are correctly written, your test should now pass 👍 Well done! You are now following the TDD development approach 💪

**[⬆️ Back to top](#-table-of-contents)**

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## 🛁 Readable Name: Variables

Mentioning clear good names and explicit names for your variables is critical to prevent confusions. Sometimes, we spend more time understanding what a variable is supposed to contain rather than achieving the given task.

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

Giving `i` for the name of your incremental variable is a terrible idea. Although, this is the standard for showing the example of the `for` loop, you should never do this in your production application (sadly, plenty of developers just repeat what they I’ve learned and we can’t blam them. It’s now time to change! Every time you declare a new variable, think about the best possible variable name.

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

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Import only what you need

Have the good practice of importing only the functions/variables you need. This will prevent against function/variable conflicts, but also optimizing your code and only expose the needed functions.

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

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## 🛁 Readable Names: Functions

### ❌ Complicated function names

```javascript
function cleaning(url) {
  // …
}
```

### ✅ Explicit descriptive names

```javascript
function removeSpecialCharactersUrl(url) {
  // …
}
```

**[⬆️ Back to top](#-table-of-contents)**

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Guard Clauses approach

The guard clauses pattern is the way of leaving a function earlier by removing the redundant `else {}` blocks after a `return` statement.

Let’s see a snippet that doesn’t follow the guard clause pattern and a clean and readable example that does.

The two samples represents the body of a function. Inside of the function we have the following 👇

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

On this example, we can noticed how we could remove the complicated nested conditionals thanks to exiting the function as early as possible with the `return` statement.

**[⬆️ Back to top](#-table-of-contents)**

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## .gitignore and .gitattributes to every project

When you are about to distribute your library, you should always create a `.gitignore` and `.gitattributes` in your project to prevent undesirable files to be presented in there.

### ✅ `.gitignore`

As soon as you commit files, your project needs a `.gitignore` file. It guarantees to exclude specific files from being committed in your codebase.

### ✅ `.gitattributes`

When you publish your package to be used by a dependency manager, it’s crucial to exclude the developing files (such as the `tests` folders, `.github` configuration folder, `CONTRIBUTING.md`, `SECURITY.me`, `.gitignore`, `.gitattributes` itself, and so on…).
Indeed, when you install a new package through your favorite package manager (npm, yarn, …), **you only want to download the required source files, that’s all** without including the test files, pipeline configuration files, etc, not needed for production.

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Demeter Law

The **demeter law**, AKA the **principle of least knowledge** states that each unit of code should only have a very limited knowledge about other units of code. They should only talk to their close friends, not to their strangers 🙃 It shouldn’t have any knowledge on the inner details of the objects it manipulates.

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

Using `console.table` saves you time as the result is shown in a clear table, improving the readability of the log when debugging an array or object.

Mozilla gives us a clear example to see how `console.table` can help you.

```javascript
const favoriteFruits = ['apple', 'bananas', 'pears'];
console.table(favoriteFruits);
```

![With console.table](https://user-images.githubusercontent.com/1325411/158784071-136971a3-8a81-448c-8c88-0c866cbcc6a8.jpeg)


**[⬆️ Back to top](#-table-of-contents)**

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Fewer arguments are winder

If your functions have more than 3 arguments, it means you could have written a much better and cleaner code. In short, the purpose of your function does too much and violates the single responsibility principle, leading to debugging and reusability hells.

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

With this version, because there is are only relevant arguments, reusing the function elsewhere will be possible as the function doesn’t rely on unnecessary arguments.

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Stub/mock only what you need

When you stub an object (with Sinon for instance), it’s a good and clean practice to decide only what functions you need to stub out, instead of stubbing out the entier object. Doing so, you also prevent overriding internal implementations of functions which cause all sorts of inconsistencies in your business logic.

### ❌ Everything is stub

```javascript
sinon.stub(classToBeStubbed);
```

### ✅ Stub only what you need

```javascript
sinon.stub(classToBeStubbed, 'myNeededFunction');
```

Here, we stub only the individual method we need.

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Import only what you need

Have the good practice of importing only the functions/variables you need. This will prevent against function/variable conflicts, but also optimizing your code and only expose the needed functions.

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

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Remove the redundant things

When we code, we always tend to write things that are just useless without increasing the readability of the code either.

For instance, having a `default` clause which isn’t used.

### ❌ Redundant

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

  default:
    break; // ❌ This is redundant
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

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Ego is your enemy ✋

Too often I see developers who take the comments on their pull requests personally, because they see what they have done as their own creation. When you receive a change request, don’t feel judged! This is actually an improvement reward for yourself 🏆

If you want to be a good developer, leave your ego in your closet. Never bring it at work. This will just slow your progression down and could even pollute your brain space and the company culture.

### ❌ Taking what others say as personally

### ✅ See every feedback as a learning experience

> When you write code, it’s not your code, it’s everybody’s else code. Don’t take what you write personally. It’s just a little part of the whole vessel.

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Don’t use abbreviations 😐

Abbreviations are hard to understand. For new joiners who aren’t familiar with the company’s terms. And even with common English abbreviations, they can be quite difficult to be understood by non-native English speakers whose might be hired in the future or when outsourcing developers from overseas.

Having as golden rule to never use abbreviations in your codebase (so at least, you’ll never have to take any further decisions on this topic) is critical for preventing confusions later on.

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
```

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## 🇺🇸 American English spelling. The default choice when coding

I always recommend to only use **US English** in your code. If you mix both British and American spellings, it will introduce some sort of confusions for later and might lead to interrogations for new developers joining the development of your software.

Most of the 3rd-party libraries are written in American English. As we use them in our codebase, we should prioritize US English as well in our code.
I’ve seen codebase with words such as “_licence_” and “_license_”, “_colour_” and “_color_”, or “_organisation_” and “_organization_”.
When you need to search for terms / refactor some code, and you find both spellings, it requires more time and consumes further brain space, which could have been avoided at the first place by following a consistent spelling.

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Destructing array elements - Make it readable

When you need to destruct an array with JavaScript (ES6 and newer), and you want to pickup only the second or third array, there is a much cleaner way than using the `,` to skip the previous array keys.

### ❌ Bad Way

```javascript
const meals = ['Breakfast', 'Lunch', 'Apéro', 'Dinner'];

const [, , , favoriteMeal] = meals;
console.log(favoriteMeal); // Dinner
```

### ✅ Recommended readable way

```javascript
const meals = ['Breakfast', 'Lunch', 'Apéro', 'Dinner'];

const { 3: favoriteMeal } = meals; // Get the 4th value, index '3'
console.log(favoriteMeal); // Dinner
```

Here, we destruct the array as an object with its index number and give an alias name `favoriteMeal` to it.

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

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Readable Names: Classes

### ❌ Generic/Vague name

```javascript
class Helper {
  // Doesn't mean anything

  stripUrl() {
    // ...
  }
  // ...
}
```

The class name is vague and doesn’t clearly communicate what it does. By reading the name, we don’t have a clear idea of what `Helper` contains and how to use it.

### ✅ Clear/Explicit name

```javascript
class Sanitizer {
  // Name is already more explicit
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
Class names should be a (singular) noun that starts with a capital letter. The class can also contain more than one noun, if so, each word has to be capitalized (this is called **UpperCamel** case).

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Avoid “else-statement”

Similar to what I mentioned concerning importance of writing portions of code that only do “**One Thing**”, you should also avoid using the _if-else statement_.

Too many times, we see code such as below with unnecessary and pointless `else` blocks.

### ❌ Conditions with unnecessary `else {}`

```javascript
const getUrl = () => {
  if (options.includes('url')) {
    return options.url;
  } else {
    return DEFAULT_URL;
  }
};
```

This code could easily be replaced by a clearer version.

### ✅ Option 1: Use default values

```javascript
const getUrl = () => {
  let url = DEFAULT_URL;

  if (options.includes('url')) {
    url = options.url;
  }
  return url;
```

By having a default value, we remove the need of a `else {}` block.

### ✅ Option 2: Use Guard Clauses approach

```javascript
const getUrl = () => {
  if (options.includes('url')) {
    return options.url; // if true, we return `options.url` and leave the function
  }
  return DEFAULT_URL;
};
```

With this approach, we leave the function early, preventing complicated and unreadable nested conditions in the future.

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

Promises make your code harder to read.

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
}
```

By using `async`/`await`, you avoid callback hell, which happens with promises chaining when data are passed through a sert of functions and leads to unmanageable code due to its nested fallbacks.

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## No magic numbers 🔢

Avoid as much as you can to hardcode changeable values which could change over time, such as the amount of total posts to display, timeout delay, and other similar information.

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

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

# Good Code™ - With JavaScript

Hi! I’m Pierre-Henry Soria. I’m a highly passionate software engineer. Originally from Brussels, (Belgium), I’m currently living in the wonderful land called “Australia” (Adelaide).

I’ve been coding for over 10 years and I decided to share my knowledge in term of writing good code.

On daily basis, I review hundreds of line of code. Brand new micro-services, new feature, new refactoring, hot fix, and so on. I’ve seen so many different coding styles as well as good and bad coding habits from the developers I’ve been working with.

With this book, you will have **the essential to know**, straight to the solution of coding better and cleaner. It’s a practical book. You won’t have superfluous information. Just the important things.
Time is so valuable and important that I only want to give you what you really need to know, without unnecessary details only there to make the book fatter.

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

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->

## Always use the strict type comparison

When doing some kind of comparison, always use the `===` strict comparison.

### ❌ Don’t use loose comparisons

```javascript
if ('abc' == true) {
} // this gives true
if (props.address != details.address) {
} // the result might not be the one you expect
```

### ✅ Use strict comparisons

```javascript
if ('abc' === true) {
} // This is false 👍
if (props.address !== details.address) {
}
```

**[⬆️ Back to top](#-table-of-contents)**

<!-- New Section (page) -->
<!-- (c) Pierre-Henry Soria -->


## About the Author

**[Pierre-Henry Soria](https://ph7.me)**. Super passionate and enthusiastic software engineer, and a true cheese & chocolate lover 💫 

[![@phenrysay](https://img.shields.io/badge/Twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white)](https://twitter.com/phenrysay) 
 [![pH-7](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/pH-7)

[![Pierre-Henry Soria](https://s.gravatar.com/avatar/a210fe61253c43c869d71eaed0e90149?s=200)](https://ph7.me "Pierre-Henry Soria personal website")
