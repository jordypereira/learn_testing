# New Technology: TDD

> For school I have to learn and document a new technology. Here's my story how I started to learn Test Driven Development. I will focus on Javascript and VueJS.

## Intro to javascript testing

_15 October 2018_

I started out watching a Tutorial from [Academind](https://youtu.be/r9HdJ8P6GQI).

### Advantages

- See errors that happen elsewhere in your code
- See a specific error when something goes wrong
- Integrate into build workflows (Pull requests etc)
- Write better code

### Kind of tests

- Unit testing: testing one isolated function
- Integration testing: test functions that use each other
- E2E testing: Simulates a user in a headless browser

### Tools needed

- Test runner: execute test and show results: Mocha
- Assertion library: define logic and conditions: Chai
- [Jest](https://jestjs.io/) is both
- Headless Browser for E2E: Puppeteer

### Unit test

- Install `jest`
- Make `.spec.js` file
- Import function that you want to test
- test(): takes a description and a callback function
- expect() this is the test

### Run the test

- jest finds all files named .test.js and .spec.js
- run `jest`
- If the test fails it prints the expected output and the received output
- add `--watch` to rerun tests when you save your code

### How to test

Test if the data is used, by using double checks with different data or test the opposite

### Integration Test

- Tests how a function that exists of other funtions work

### E2E Testing

- Install `puppeteer`
- You can find all the commands in the [doc](https://github.com/GoogleChrome/puppeteer)
- This will run a browser and click buttons or type text

## Let's try a TDD Tutorial

_20 October 2018_

I found a great [article](https://medium.com/magnetis-backstage/working-an-application-in-vue-js-with-tdd-an-extensive-guide-for-people-who-have-time-part-1-3be791dafa2b) with a step by step tutorial, making a simple Vue app in TDD. Test Driven Development is when you write tests before the component and keep testing as you make it.

I documented me following the tutorial [here](./TDD-Tutorial/)

- Test files are called .spec.js
- vue-test-utils gives methods to load vue components, mainly `mount()` and `shallowMount()`
- You make a simple test to mount a component, run the test and it will tell you that the component doesn't exist. Now we make the component.
- Snapshots compare HTML structure.

## Listening to the pro's

_3 November 2018_

Today I watched [Edd Yerburgh - Unit testing Vue components Why test, what to test, and how to test Vue components](https://youtu.be/LxXsGNXsMo8), from the creator of vue-test-utils.

[Jest](https://vue-test-utils.vuejs.org/guides/testing-single-file-components-with-jest.html) is easy to set up. I go briefly over his talk [here](./Edd-Yerburgh/).

He sums up the benefits of testing, how to get started quickly, what to test and what to expect.

## Academind tests async JS code

In this [video](https://www.youtube.com/watch?v=4Fl5GH4eYZ8) he'll be mocking async code.

You can just call the api function and then compare the output to what you think the output should be. But you don't want to call an api in a test.
You don't test if the API works in your frontend.
So we have to mock test. This is where we replace other packages with dummy content.  
Create `__mocks__` folder where you remake the fetch function with dummy data that the api returns.

```js
// __mocks__/http.js
const fetchData = () => {
  return Promise.resolve({ title: 'delectus aut autem' })
}
exports.fetchData = fetchData
```

Now if you call `jest.mock('./http.js')` jest will replace the real http function with the mocked function.  
Now to mock axios you can make an axios.js file. Jest will automatically use this because axios is a third party library.

```js
export const get = url => {
  return { data: { title: 'delectus aut autem' } }
}
```

You just replace the function with your own version.
