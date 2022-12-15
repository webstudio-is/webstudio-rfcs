- Start Date: (2022-12-07)
- RFC PR: (leave this empty)
- Issue: (leave this empty)

# Summary

As a contributor to Webstudio codebases we need to agree when to use `undefined` and when to use `null`.

Prior art:

- [Article](https://oleg008.medium.com/what-if-we-stop-using-null-d705302b545e)
- [PR discussion](https://github.com/sindresorhus/meta/discussions/7)
- [Presentation by Douglas Clockford](https://www.youtube.com/watch?v=PSGEjv3Tqo0&t=561s)
- [Tweet from creattor of JavaScript](https://twitter.com/brendaneich/status/617450289889607681) and [this](https://twitter.com/brendaneich/status/330775086208524288)
- [Internal TypeScript convention for the compiler itself](https://github.com/microsoft/TypeScript/wiki/Coding-guidelines#null-and-undefined)

# Motivation

Web platform uses both and both have their advantages and downsides, so we want to decide which adavntages we prefer over disadvantages.

Advatages to use `undefined`:

- Default function parameters (JavaScript does not support `null` with default arguments)

  ```js
  const f = (a = 1) => a;
  f(); // 1
  f(undefined); // 1
  f(null); // null
  ```

- Destructuring with default values

  ```js
  const { a = 1 } = { a: null };
  a; // null
  const { a = 1 } = {};
  a; // 1
  const { a = 1 } = { a: undefined };
  a; // 1
  const [a = 1] = [];
  a; // 1
  const [a = 1] = [undefined];
  a; // 1
  const {
    a: { b = 1 },
  } = { a: { b: undefined } };
  b; // 1
  ```

- Using null makes TypeScript types more verbose: `type A = {foo?: string | null}` vs `type A = {foo?: string}`.

- `typeof null === 'object'`

# Detailed design

- Use `undefined` by default 99% of the time. Only use `null` when you can't use `undefined`.
- Using `return;` or returning nothing is fine
- Use `null` when you need to indicate a `delete` operation over serialized JSON, since undefined can't be serialized with JSON.stringify and will be omitted.
- Keep `null` usages local, don't return null to consumer functions to avoid spreading usage of `null`.
- If you don't have a value, omit it instead of setting it to `null` or `undefined`.
- Don't create interfaces that support both values, because it may lead to more complex logic once those values need validation or being passed to other functions
- If for some external reason you are forced to support both (external API), use `== null` check with double equal, this way you can concisely check for both, otherwise always use `===`.
- Using `== null` when the value can only be `undefined` creates indirection.

# Drawbacks

- Cases where we can't use `undefined`:

  - Explicitely express a deletion of a value using `undefined`.
  - We can't serialize with JSON.stringify() `undefined`

  In both isolated cases we can use `null`.

- DOM APIs and some React APIs are using `null`.
  We will have to keep using `null` in such cases in an isolated scenario, but avoid returning it to the consumers.

# Alternatives

Alternative would be to always use `null` and stick with it consistently everywhere.

Unfortunately disadvantages are strong.

# Adoption strategy

So far adoption was enforced by code reviews, but with more contributors we may have to add a linter.

For now we just have to agree on the usage and try to do it consistently.

# Unresolved questions

Optional, but suggested for first drafts. What parts of the design are still TBD?
