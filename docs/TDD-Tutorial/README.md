# TDD-Tutorial

## Episode 1 / 6

### Intro

We are going to make a Simple app that speaks with the Github API. You can search a [user](https://developer.github.com/v3/users/) and it shows its profile.  
The challenge: Only run yarn serve when our application is finished.

Create the app using the vue cli and select the Jest testing solution.

Add `"test:unit:watch": "vue-cli-service test:unit --watchAll"` to your startup scripts.  
Install `axios`, `flush-promises` and `nock` in dev mode.

### Create tests for the smart component Userview

We make a file called `tests/unit/component.spec.js`. Which will be recognized by Jest.

Test for these things:

- If the component renders
- If it renders the right thing
- Props passed
- Events handled
- All possible cases of the component

#### shallowMount or mount from vue-test-utils

You import the component and a way to load it, in this case shallowMount from the vue-test-utils. Shallow Mount only renders the first level of the component, so no children. The other option is Mount, which renders everything.

#### Jest

```js
import { shallowMount } from '@vue/test-utils'
import UserView from '@/views/UserView'

describe('UserView', () => {
  it('renders the component', () => {
    // arrange
    const wrapper = shallowMount(UserView)

    // assert
    expect(wrapper.html()).toMatchSnapshot()
  })
})
```

You can use test instead of it. It is more widely used.

Now if we test this we get the error that the component does not exist. So we make that after we failed our first test. TDD.

#### Snapshots

the toMatchSnapshot first makes a snapshot of the rendered HTML and compares if it matches the snapshot it made.
If it doesn't, you get a nice feedback explaining what changed.

```html
      <div>
    -   UserView
    +   Hello World
      </div>
```

When running the watch command, you can press 'u' to update all snapshots.

## Episode 2 / 6

### Renders important components

We want to check if the Userview renders the right components. Add the second test to the describe function.

```js
it('renders main child components', () => {
  const wrapper = shallowMount(UserView)
  const userSearchForm = wrapper.find(VUserSearchForm)
  const userProfile = wrapper.find(VUserProfile)

  expect(userSearchForm.exists()).toBe(true)
  expect(userProfile.exists()).toBe(true)
})
```

the [find is a vue-test-utils](https://vue-test-utils.vuejs.org/api/wrapper/#find-selector) method which we can then check if it [exists](https://vue-test-utils.vuejs.org/api/wrapper/#exists).

After the test failed we make the components and update the snapshot. If you use the components in the `UserView` it will pass the test.

### Build function

He puts functions that's he's gonna use a lot in a build function.

```js
const build = () => {
  const wrapper = shallowMount(UserView)

  return {
    wrapper,
    userSearchForm: () => wrapper.find(VUserSearchForm),
    userProfile: () => wrapper.find(VUserProfile),
  }
}
```

### Test if it receives the right props

Vue utils let's you pass data to the wrapper:

```js
const wrapper = shallowMount(UserView, {
  data: () => ({
    user: {},
  }),
})
```

Then we can declare a new test to pollute this data

```js
it('passes a binded user prop to user profile component', () => {
  // arrange
  const { wrapper, userProfile } = build()
  wrapper.setData({
    user: {
      name: 'Daniel',
    },
  })

  // assert
  expect(userProfile().vm.user).toBe(wrapper.vm.user)
})
```

Here we can compare the user of our wrapper to the userProfile.
To make the test pass, we need to bind the user to userProfile and make it receive the prop.

# Episode 3 / 6

## Test if it receives the store

Now to use a store in our tests we have to inject it the same way as we do in any vue instance. We create a local vue instance and inject vuex into that.

```js
import { shallowMount, createLocalVue } from '@vue/test-utils'
import Vuex from 'vuex'

const localVue = createLocalVue()
localVue.use(Vuex)

let state

const wrapper = shallowMount(UserView, {
  localVue,
  store: new Vuex.Store({ state }),
})
```

And reset the state after each test

```js
import initialState from '@/store/state'

beforeEach(() => {
  state = { ...initialState }
})
```

We change our component to use the vuex user and change the test to compare it with the vuex user.

```js
it('passes a binded user prop to user profile component', () => {
  // arrange
  state.user = userFixture
  const { userProfile } = build()

  // assert
  expect(userProfile().vm.user).toBe(state.user)
})
```
