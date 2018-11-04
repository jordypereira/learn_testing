# Edd Yerburgh - Unit testing Vue components Why test, what to test, and how to test Vue components

Edd Yerburgh is an experienced JavaScript developer and Vue core contributor. He is the main author of the official Vue test library and a prominent figure in the Vue testing community.

Today I watched his talk [Edd Yerburgh - Unit testing Vue components Why test, what to test, and how to test Vue components](https://youtu.be/LxXsGNXsMo8)

Testing can start out easy. We'll be using [Jest](https://vue-test-utils.vuejs.org/guides/testing-single-file-components-with-jest.html)

```js
import sum from './sum';

test('returns sum of input', () => {
  expect(sum(1, 3)).toBe(4);
});
```

You run a function and say what the answer is. This equals either true or false and decides if the test passed or failed.

## The benefits of unit testing

- Check if components you haven't touched still work
- Provides documentation because it explains what it's supposed to do
- Easier debugging because it tells you what doesn't work

Vue components are not javascript so we need to compile them. This can be done with webpack or vue-jest.  
Install `jest` `vue-jest` `babel-jest` to compile vue code and js code for the jest tests. These are auto installed when using the vue cli 3 plugin [@vue/cli-plugin-unit-jest](https://github.com/vuejs/vue-cli/tree/dev/packages/@vue/cli-plugin-unit-jest#readme).

```json
// package.json

{
  // ..
  "jest": {
    "transform": {
      "^.+\\.js$": "babel-jest",
      "^.+\\.vue$": "vue-jest"
    }
  }
  // ..
}
```

This code uses the transformer. Vue CLI 3 adds this in `jest.config.js`.

Run the tests with `jest`. To test Vue components we again need `vue@test-utils`. This lets use the `mount()` that we can store in a `const wrapper`. This exposes the wrapper.vm adn the wrapper.element.

## What to test

- User action
- Props
- Store

## What to get

- Rendered output
- Vue events
- Function calls

## Vue test utils

Create your component with the functions from vue test utils and jest.

## Snapshot tests

Makes a snapshot of the HTML which you can compare with in the future.
Format the tests with `jest-serializer-vue`.
Pass `-u` to reset the snapshot.
