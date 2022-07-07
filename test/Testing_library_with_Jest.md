# Testing library with Jest

## Unit testing

is for test a peace of code

### Integration test

check if multiple unit testing are working great together. Combine multiples unit testing into one test

## End to End

test to ends (front to backend) are working great is most similar to a real case

Jest ⇒ test Runner

Testing library ⇒ create a virtual DOM to manipulate,searching and  inside Jest

Simple test example:

```jsx
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders learn react link', () => {
  render(<App />);
	// Search inside the DOM a text based of regular expression or string if exist backs the HTML element
  const linkElement = screen.getByText(/learn react/i);
	//Assertion expert pass a HTMLElement and toBeInTheDocument (matcher) search in the DOM
  expect(linkElement).toBeInTheDocument();
});
```

this test run first the render method.  this method creates a virtual DOM based of component.

you can access to this virtual DOM via screen global.

## Assertion

Expect = Jest global variable , starts an assertion. assertion can be an array or element more info about expect [https://jestjs.io/docs/expect](https://jestjs.io/docs/expect) .

Matcher = type of assertion this matches comes from Jest DOM ex : toBeInTheDocument

if you want to see more matchers [https://jestjs.io/docs/using-matchers#common-matchers](https://jestjs.io/docs/using-matchers#common-matchers)

### Examples:

```jsx
expect(element.textContent).toBe('hello');
expect(elementsArray).toHaveLength(7);
expect(element).toBeVisible();
expect(element).toBeChecked();
```

expect functions returns an expectation object and follow the matchers if it sends a error

## Jest-dom

Comes with create-react-app in ‘src/setupsTest.js’ imports it before each test , makes  matchers available . 

This package comes from testing-library it gives you custom jest matchers to test the state of the DOM inside your Jest Tests. Soma matchers examples: 

- `[toBeDisabled](https://github.com/testing-library/jest-dom#tobedisabled)`
- `[toBeRequired](https://github.com/testing-library/jest-dom#toberequired)`
- `[toHaveStyle](https://github.com/testing-library/jest-dom#tohavestyle)`

If you want more information about the matchers you con find the documentation in : [https://github.com/testing-library/jest-dom](https://github.com/testing-library/jest-dom)

## How Jest Works

Jest have a global object his name is test and this method accepts two arguments:

- string (description)
- function (test)

if the test doesn’t have a error this test is mark success if some assertion throw a error it marks false

## Testing library philosophy

Unit test : test one unit  of code isolation very easy to pinpoint failures 

Integration test : How multiple units work together 

functional test : test a particular function of software. Close how users interact and robust tests

Acceptance / End to end(E2E) tests : use the browser and server (Cypress, Selenium, etc)

## Describe

Its a group of test 

## fireEvent

Is part of testing library it helps you to firing DOM Events his basic syntax is 

```jsx
// fireEvent[eventName](node: HTMLElement, eventProperties: Object)
const confirmButton = screen.getByRole('button',
    {
      name: /confirm order/i
    }
  );
fireEvent.click(confirmButton)

```

T**arget**
: When an event is dispatched on an element, the event has the subjected element on a property called `target`
. As a convenience, if you provide a `target`
 property in the `eventProperties`
 (second argument), then those properties will be assigned to the node which is receiving the event.

Example:

```jsx
fireEvent.change(getByLabelText(/username/i), {target: {value: 'a'}})
```

## **user-event**

Is a companion library for Testing Library that provides more advanced simulation of browser interactions than the built-in fireEvent method.

## Screen

All of the queries exported by DOM Testing Library accept a `container` as the first argument. Because querying the entire `document.body` is very common, DOM Testing Library also exports a `screen` object which has every query that is pre-bound to `document.body` with this method you can get HTMLElements inside your created dom. 

for searching methods you can check [here](https://testing-library.com/docs/queries/about)

Command[all]ByQueryType

Commands

- get: expect element to be in DOM
- query: expect element not to be in DOM
- find: expect element to appear async

[all] ⇒ exclude (expect only one match) or include (expect more than one match)

## Type query

- Single elements
    - `getBy...`: Returns the matching node for a query, and throw a descriptive error if no elements match *or* if more than one match is found (use `getAllBy` instead if more than one element is expected).
    - `queryBy...`: Returns the matching node for a query, and return `null` if no elements match. This is useful for asserting an element that is not present. Throws an error if more than one match is found (use `queryAllBy` instead if this is OK).
    - `findBy...`: Returns a Promise which resolves when an element is found which matches the given query. The promise is rejected if no element is found or if more than one element is found after a default timeout of 1000ms. If you need to find more than one element, use `findAllBy`.
- Multiple Elements
    - `getAllBy...`: Returns an array of all matching nodes for a query, and throws an error if no elements match.
    - `queryAllBy...`: Returns an array of all matching nodes for a query, and return an empty array (`[]`) if no elements match.
    - `findAllBy...`: Returns a promise which resolves to an array of elements when any elements are found which match the given query. The promise is rejected if no elements are found after a default timeout of `1000`ms.
        - `findBy` methods are a combination of `getBy*` queries and `[waitFor](https://testing-library.com/docs/dom-testing-library/api-async#waitfor)`. They accept the `waitFor` options as the last argument (i.e. `await screen.findByText('text', queryOptions, waitForOptions)`)

| Type of query | 0 Matches | 1 Match | >1 Matches | Retry (async/await) |
| --- | --- | --- | --- | --- |
| Single element |  |  |  |  |
| getBy... | Throw error | Return Element | Throw error | No |
| queryBy... | Return null | Return Element | Throw error | No |
| findBy... | Throw error | Return Element | Throw error | Yes |
| Multiple Elements |  |  |  |  |
| getAllBy... | Throw error | Return Array | Return Array | No |
| queryAllBy... | Return [] | Return Array | Return Array | No |
| findAllBy... | Throw error | Return Array | Return Array | Yes |

## Priority

- Role (most preferred)
- altText (images
- text (display elements)
- Form elements:
    - placeholderText
    - labelText
    - displayValue

## **Firing Events**

Is a package of testing library it helps you to do user actions like a click 

usage:

```jsx
fireEvent(node: HTMLElement, event: Event)
```

```jsx
//container is the HTMLElement MouseEvent is 
//the event create with the mouse withis his options
fireEvent(
  getByText(container, 'Submit'),
  new MouseEvent('click', {
    bubbles: true,
    cancelable: true,
  }),
);

```

if you want to know the event type you can access to this link [src/event-map.js](https://github.com/testing-library/dom-testing-library/blob/master/src/event-map.js)

## Links

[https://testing-library.com/docs/react-testing-library/cheatsheet/](https://testing-library.com/docs/react-testing-library/cheatsheet/)

[https://testing-library.com/docs/queries/about/#priority](https://testing-library.com/docs/queries/about/#priority)