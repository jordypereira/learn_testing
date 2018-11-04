# TDD-Tutorial

## Episode 1 / 6

### Intro

We are going to make a Simple app that speaks with the Github API. You can search a [user](https://developer.github.com/v3/users/) and it shows its profile.  
The challenge: Only run yarn serve when our application is finished.

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
import { shallowMount } from '@vue/test-utils';
import UserView from '@/views/UserView';

describe('UserView', () => {
  it('renders the component', () => {
    // arrange
    const wrapper = shallowMount(UserView);

    // assert
    expect(wrapper.html()).toMatchSnapshot();
  });
});
```

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
