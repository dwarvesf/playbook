# Writing test

Testing your application is a vital part of serious development. For the front-end, you don't need 100% code coverage, about 70% is probably good enough. Following are some principles:

- _Write tests. Not too many. Mostly integration_.
- Tests should make you more productive not slow you down. Maintaining tests can slow you down. You get dimishing returns on adding more tests after a certain point.
- Your tests should always resemble the way your software is used.
- Make sure that you're not testing implementation details - things which users do not use, see, or even know about.
- If your tests don't make you confident that you didn't break anything, then they didn't do their (one and only) job.
- You'll know you implemented correct tests when you rarely have to change tests when you refactor code given the same user behavior.
- Use [Jest](https://jestjs.io/), [React testing library](https://testing-library.com/docs/react-testing-library/intro/), [Cypress](https://www.cypress.io/), and [Mock service worker](https://github.com/mswjs/msw).

## Unit testing

Unit testing is the practice of testing the smallest possible units of our code, functions. We run our tests and automatically verify that our functions do the thing we expect them to do. We assert that, given a set of inputs, our functions return the proper values and handle problems.

We use the Jest test framework to run tests and make assertions. This library makes writing tests as easy as speaking - you describe a unit of your code and expect it to do the correct thing.

For the sake of this guide, lets pretend we're testing this function. It's situated in the `number.ts` file:

```js
export function add(x: number, y: number) {
  return x + y;
}
```

First create a folder call `tests` then add a second `add.test.ts` file with our unit test inside.

```js
import { add } from '../number.ts';

describe('add()', () => {
  it('adds two numbers', () => {
    expect(add(2, 3)).toEqual(5);
  });

  it("doesn't add the third number", () => {
    expect(add(2, 3, 5)).toEqual(add(2, 3));
  });
});
```

Should our function work, Jest will show this output when running the tests:

```bash
add()
  ✓ adds two numbers
  ✓ doesn't add the third number
```

Oh no, now our function doesn't add the numbers anymore, it multiplies them! Imagine the consequences to our code that uses the function!

Thankfully, we have unit tests in place. Because we run the unit tests before we deploy our application, we see this output:

```
● add() › adds two numbers

  expect(received).toEqual(expected)

  Expected value to equal:
    5
  Received:
    6

add()
  ✕ adds two numbers
  ✓ doesn't add the third number
```

## Component testing

Unit testing your utils and hooks is nice, but you can do even more to make sure nothing breaks your application. Since React is the view layer of your app, let's see how to test Components too!

### Shallow rendering

React provides us with a nice add-on called the Shallow Renderer. This renderer will render a React component **one level deep**. Lets take a look at what that means with a simple `<Button>` component.

This component renders a `<button>` element containing a checkmark icon and some text:

```javascript
import React from 'react';
import CheckmarkIcon from './CheckmarkIcon';

function Button(props) {
  return (
    <button className="btn" onClick={props.onClick}>
      <CheckmarkIcon />
      {React.Children.only(props.children)}
    </button>
  );
}

export default Button;
```

It might be used in another component like this:

```javascript
import Button from './Button';

function HomePage() {
  return <Button onClick={this.doSomething}>Click me!</Button>;
}
```

When rendered normally with the standard `ReactDOM.render` function, this will be the HTML output (_Comments added in parallel to compare structures in HTML from JSX source_):

```html
<button>
  <!-- <Button>             -->
  <i class="fa fa-checkmark"></i>
  <!--   <CheckmarkIcon />  -->
  Click Me!
  <!--   { props.children } -->
</button>
<!-- </Button>            -->
```

Conversely, when rendered with the shallow renderer, we'll get a String containing this "HTML":

```html
<button>
  <!-- <Button>             -->
  <CheckmarkIcon />
  <!--   NOT RENDERED!      -->
  Click Me!
  <!--   { props.children } -->
</button>
<!-- </Button>            -->
```

If we test our `Button` with the normal renderer and there's a problem with the `CheckmarkIcon` then the test for the `Button` will fail as well. This makes it harder to find the culprit. Using the _shallow_ renderer, we isolate the problem's cause since we don't render any other components other than the one we're testing!

The problem with the shallow renderer is that all assertions have to be done manually, and you cannot do anything that needs the DOM.

### react-testing-library

In order to write more maintainable tests which also resemble more closely the way our component is used in real life, we have included [react-testing-library](https://github.com/testing-library/react-testing-library). This library renders our component within an actual DOM and provides utilities for querying it.

Let's give it a go with our `<Button />` component, shall we? First, let's check that it renders our component with its children, if any, and second that it handles clicks.

This is our test setup:

```javascript
import React from 'react';
import { render, fireEvent } from 'react-testing-library';
import Button from '../Button';

describe('<Button />', () => {
  it('renders and matches the snapshot', () => {});

  it('handles clicks', () => {});
});
```

#### Snapshot testing

Let's start by ensuring that it renders our component and no changes happened to it since the last time it was successfully tested.

We will do so by rendering the component and creating a _[snapshot](https://jestjs.io/docs/en/snapshot-testing)_ which can be compared with a previously committed snapshot. If no snapshot exists, a new one is created.

For this, we first call `render`. This will render our `<Button />` component into a _container_, by default a `<div>`, which is appended to `document.body`. We then create a snapshot and `expect` that this snapshot is the same as the existing snapshot, taken in a previous run of this test and committed to the repository.

```javascript
it('renders and matches the snapshot', () => {
  const text = 'Click me!';
  const { container } = render(<Button>{text}</Button>);

  expect(container.firstChild).toMatchSnapshot();
});
```

`render` returns an object that has a property `container` and yes, this is the container our `<Button />` component has been rendered in.

As this is rendered within a _normal_ DOM we can query our component with `container.firstChild`. This will be our subject for a snapshot. Snapshots are placed in the `__snapshots__` folder within our `tests` folder. Make sure you commit these snapshots to your repository.

So, now if anyone makes any change to our `<Button />` component the test will fail and we get notified of what changed.

**Further reading:**

- [Why snapshot testing?](https://jestjs.io/blog/2016/07/27/jest-14.html#why-snapshot-testing)

#### Behavior testing

Onwards to our last and most advanced test: checking that our `<Button />` handles clicks correctly.

We'll use a [mock function](https://jestjs.io/docs/en/mock-functions) for this. A mock function is a function that keeps track of _if_, _how often_, and _with what arguments_ it has been called. We pass this function as the `onClick` handler to our component, simulate a click and, lastly, check that our mock function was called:

```javascript
it('handles clicks', () => {
  const onClickMock = jest.fn();
  const text = 'Click me!';
  const { getByText } = render(<Button onClick={onClickMock}>{text}</Button>);

  fireEvent.click(getByText(text));
  expect(onClickSpy).toHaveBeenCalledTimes(1);
});
```

Our finished test file looks like this:

```javascript
import React from 'react';
import { render, fireEvent } from 'react-testing-library';
import Button from '../Button';

describe('<Button />', () => {
  it('renders and matches the snapshot', () => {
    const text = 'Click me!';
    const { container } = render(<Button>{text}</Button>);

    expect(container.firstChild).toMatchSnapshot();
  });

  it('handles clicks', () => {
    const onClickMock = jest.fn();
    const text = 'Click me!';
    const { getByText } = render(<Button onClick={onClickMock}>{text}</Button>);

    fireEvent.click(getByText(text));
    expect(onClickSpy).toHaveBeenCalledTimes(1);
  });
});
```

## Integration testing

The unit and component testing might ensure the single responisibilty of a function or a UI component but we cannot guarantee they work as expected when being integrated as a whole. That's why integration testing comes in to confirm that an aggregate of a system works together correctly or otherwise exposes errorous behavior between two or more units of code.

You can choose to write integration tests by using [React testing library](https://testing-library.com/docs/react-testing-library/intro/) or [Cypress](https://www.cypress.io/). The test should aim to resemble user's persepective, how users access information on the app and interact with available controls in a stimulate browser. Below is an example of testing the sign-up flow with Cypress:

```js
describe('User Sign-up and Login', () => {
  it('should redirect unauthenticated user to login page', () => {
    cy.visit('/');
    cy.location('pathname').should('equal', '/login');
  });

  it('should redirect unauthenticated user to login page', function () {
    cy.visit('/forms');
    cy.location('pathname').should('equal', '/login');
  });

  it('should allow a visitor to sign-up, login, and logout', function () {
    const userInfo = {
      email: 'test@d.foundation',
      password: 'test',
    };

    cy.visit('/');
    cy.get('[name="email"]').type(userInfo.email);
    cy.get('[name="password"]').type(userInfo.password).type('{enter}');
    cy.location('pathname').should('equal', '/');
    cy.get('[data-testid="profile-button"]').click();
    cy.get('[data-testid="logout-button"]').click();
    cy.location('pathname').should('equal', '/login');
  });
});
```

If the test requires API calls, make sure they are mocked up to work **without the need of a real server**.

Here's an example of an integration test that uses [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/) and [Mock Service Worker](https://github.com/mswjs/msw):

```js
import { setupWorker, rest } from 'msw';

const worker = setupWorker(
  rest.post('/login', (req, res, ctx) => {
    return res(
      ctx.delay(1500),
      ctx.status(202, 'Mocked status'),
      ctx.json({
        message: 'Mocked response JSON body',
      }),
    );
  }),
);

worker.start();
```

And here's how to use Cypress and intercept the network request for an integration test:

```js
// requests to '/login' will be fulfilled
// with a body of "success"
cy.intercept('/login', 'Mocked response JSON body');
```

Note: sometimes, it's required to have e2e tests by making requests against a real backend server. In that case, we need to ensure network latency and availability of the server are not the factor causing the test to fail.

## Read on:

- [Tech ecosystem](tech-ecosystem.md)
- [Code style](code-style.md)
- [UI checklist](ui-checklist.md)
- [Logging and monitoring](logging-monitoring.md)
