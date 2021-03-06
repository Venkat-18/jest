---
id: using-matchers
title: Using Matchers
layout: docs
category: Introduction
permalink: docs/using-matchers.html
previous: getting-started
next: asynchronous
---

Jest uses "matchers" to let you test values in different ways. There are too many different matchers to memorize them all, so this document will only try to introduce the most useful ones.

### Common Matchers

The simplest way to test a value is with exact equality.

```js
test('two plus two is four', () => {
  expect(2 + 2).toBe(4);
})
```

In this code, `expect(2 + 2)` returns an "expectation" object. You typically won't do much with these expectation objects except call matchers on them. In this code, `.toBe(4)` is the matcher. When Jest runs, it tracks all the failing matchers so that it can print out nice error messages for you.

`toBe` uses `===` to test exact equality. If you want to check the value of an object, use `toEqual` instead:

```js
test('object assignment', () => {
  let data = { one: 1 };
  data['two'] = 2;
  expect(data).toEqual({ one: 1, two: 2 });
})
```

`toEqual` recursively checks every field of an object or array.

You can also test for the opposite of a matcher:

```js
test('adding positive numbers is not zero', () => {
  for (let a = 1; a < 10; a++) {
    for (let b = 1; b < 10; b++) {
      expect(a + b).not.toBe(0);
    }
  }
})
```

### Truthiness

In tests you sometimes need to distinguish between `undefined`, `null`, and `false`, but you sometimes do not want to treat these differently. Jest contains helpers that let you be explicit about what you want.

* `toBeNull` matches only `null`
* `toBeUndefined` matches only `undefined`
* `toBeDefined` is the opposite of `toBeUndefined`
* `toBeTruthy` matches anything that an `if` statement treats as true
* `toBeFalsy` matches anything that an `if` statement treats as false

For example:

```js
test('null', () => {
  let n = null;
  expect(n).toBeNull();
  expect(n).toBeDefined();
  expect(n).not.toBeUndefined();
  expect(n).not.toBeTruthy();
  expect(n).toBeFalsy();
});

test('zero', () => {
  let z = 0;
  expect(z).not.toBeNull();
  expect(z).toBeDefined();
  expect(z).not.toBeUndefined();
  expect(z).not.toBeTruthy();
  expect(z).toBeFalsy();
});
```

You should use the matcher that most precisely corresponds to what you want your code to be doing.

### Numbers

Most ways of comparing numbers have matcher equivalents.

```js
test('two plus two', () => {
  let value = 2 + 2;
  expect(value).toBeGreaterThan(3);
  expect(value).toBeGreaterThanOrEqual(3.5);
  expect(value).toBeLessThan(5);
  expect(value).toBeLessThanOrEqual(4.5);

  // toBe and toEqual are equivalent for numbers
  expect(value).toBe(4);
  expect(value).toEqual(4);
});
```

For floating point equality, use `toBeCloseTo` instead of `toEqual`, because you don't want a test to depend on a tiny rounding error.

```js
test('adding floating point numbers', () => {
  let value = 0.1 + 0.2;
  expect(value).not.toBe(0.3);    // It isn't! Because rounding error
  expect(value).toBeCloseTo(0.3); // This works.
})
```

### Strings

You can check strings against regular expressions with `toMatch`:

```js
test('there is no I in team', () => {
  expect('team').not.toMatch(/I/);
});

test('but there is a "stop" in Christoph', () => {
  expect('Christoph').toMatch(/stop/);
})
```

### Arrays

You can check if an array contains a particular item using `toContain`:

```js
let shoppingList = ['diapers', 'kleenex', 'trash bags', 'paper towels', 'beer'];

test('the shopping list has beer on it', () => {
  expect(shoppingList).toContain('beer');
})
```

### Exceptions

If you want to test that a particular function throws an error when it's called, use `toThrow`.

```js
function compileAndroidCode() {
  throw new ConfigError('you are using the wrong JDK');
}

test('compiling android goes as expected', () => {
  expect(compileAndroidCode).toThrow();
  expect(compileAndroidCode).toThrow(ConfigError);

  // You can also use the exact error message or a regexp
  expect(compileAndroidCode).toThrow('you are using the wrong JDK');
  expect(compileAndroidCode).toThrow(/JDK/);
})
```

### And More

This is just a taste. For a complete list of matchers, check out the [reference docs](/jest/docs/expect.html).

Once you've learned about the matchers that are available, a good next step is to check out how Jest lets you [test asynchronous code](/jest/docs/asynchronous.html).
