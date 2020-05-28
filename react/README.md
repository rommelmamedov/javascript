This style guide is mostly based on the standards that are currently prevalent in JavaScript, although some conventions (i.e async/await or static class fields) may still be included or prohibited on a case-by-case basis. Currently, anything prior to stage 3 is not included nor recommended in this guide.

## Table of Contents

1. Basic Rules
2. Class vs stateless
3. Mixins
4. Naming
6. Declaration
8. Alignment
10. Quotes
12. Spacing
14. Props
16. Refs
18. Parentheses
20. Tags
22. Methods
24. Ordering
26. `isMounted`
27. Project Folder Sturcture

## Basic Rules

- Only include one React component per file.
    - However, multiple [Stateless, or Pure, Components](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) are allowed per file. eslint: `[react/no-multi-comp](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless)`.
- Always use JSX syntax.
- Do not use `React.createElement` unless you’re initializing the app from a file that is not JSX.
- `[react/forbid-prop-types](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/forbid-prop-types.md)` will allow `arrays` and `objects` only if it is explicitly noted what `array` and `object` contains, using `arrayOf`, `objectOf`, or `shape`.

## Class vs stateless

- Always prefer arrow functions over class based components:

    ```jsx
    // Bad!
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>;
      }
    }

    // Bad!
    function Listing({ hello }) {
      return <div>{hello}</div>;
    }

    // Good
    const Listing = ({ hello }) => <div>{hello}</div>;

    ```

## Mixins

- [Do not use mixins](https://facebook.github.io/react/blog/2016/07/13/mixins-considered-harmful.html).

> Why? Mixins introduce implicit dependencies, cause name clashes, and cause snowballing complexity. Most use cases for mixins can be accomplished in better ways via components, higher-order components, or utility modules.

## Naming

- **Extensions**: Use `.js` extension for React components.
- **Filename**: Use PascalCase for filenames. E.g., `ReservationCard.js`.
- **Reference Naming**: Use PascalCase for React components and camelCase for their instances. eslint: `[react/jsx-pascal-case](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)`

    ```jsx
    // Bad!
    import reservationCard from './ReservationCard';

    // Good
    import ReservationCard from './ReservationCard';

    // Bad!
    const ReservationItem = <ReservationCard />;

    // Good
    const reservationItem = <ReservationCard />;
    ```

- **Component Naming**: Use the filename as the component name. For example, `ReservationCard.js` should have a reference name of `ReservationCard`. However, for root components of a directory, use `index.js` as the filename and use the directory name as the component name:

    ```jsx
    // Bad!
    import Footer from './Footer/Footer';

    // Bad!
    import Footer from './Footer/index';

    // Good
    import Footer from './Footer';
    ```

- **Higher-order Component Naming**: Use a composite of the higher-order component’s name and the passed-in component’s name as the `displayName` on the generated component. For example, the higher-order component `withFoo()`, when passed a component `Bar` should produce a component with a `displayName` of `withFoo(Bar)`.

    > Why? A component’s displayName may be used by developer tools or in error messages, and having a value that clearly expresses this relationship helps people understand what is happening.

    ```jsx
    // Bad!
    export default function withFoo(WrappedComponent) {
      return function WithFoo(props) {
        return <WrappedComponent {...props} foo />;
      }
    }

    // Good
    export default function withFoo(WrappedComponent) {
      function WithFoo(props) {
        return <WrappedComponent {...props} foo />;
      }

      const wrappedComponentName = WrappedComponent.displayName
        || WrappedComponent.name
        || 'Component';

      WithFoo.displayName = `withFoo(${wrappedComponentName})`;
      return WithFoo;
    }
    ```

- **Props Naming**: Avoid using DOM component prop names for different purposes.

    > Why? People expect props like style and className to mean one specific thing. Varying this API for a subset of your app makes the code less readable and less maintainable, and may cause bugs.

    ```jsx
    // Bad!
    <MyComponent style="fancy" />

    // Bad!
    <MyComponent className="fancy" />

    // Good
    <MyComponent variant="fancy" />
    ```

## Declaration

- Do not use `displayName` for naming components. Instead, name the component by reference.

    ```jsx
    // Bad!
    export default React.createClass({
      displayName: 'ReservationCard',
      // stuff goes here
    });

    // Good
    export default class ReservationCard extends React.Component {
    }
    ```

## Alignment

- Follow these alignment styles for JSX syntax. eslint: `[react/jsx-closing-bracket-location](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)` `[react/jsx-closing-tag-location](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-tag-location.md)`

    ```jsx
    // Bad!
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // Good
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    />

    // if props fit in one line then keep it on the same line
    <Foo bar="bar" />

    // children get indented normally
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    >
      <Quux />
    </Foo>

    // Bad!
    {showButton &&
      <Button />
    }

    // Bad!
    {
      showButton &&
        <Button />
    }

    // Good
    {showButton && (
      <Button />
    )}

    // Good
    {showButton && <Button />}
    ```

## Quotes

- Always use double quotes (`"`) for JSX attributes, but single quotes (`'`) for all other JS. eslint: `[jsx-quotes](https://eslint.org/docs/rules/jsx-quotes)`

    > Why? Regular HTML attributes also typically use double quotes instead of single, so JSX attributes mirror this convention.

    ```jsx
    // Bad!
    <Foo bar='bar' />

    // Good
    <Foo bar="bar" />

    // Bad!
    <Foo style={{ left: "20px" }} />

    // Good
    <Foo style={{ left: '20px' }} />
    ```

## Spacing

- Always include a single space in your self-closing tag. eslint: `[no-multi-spaces](https://eslint.org/docs/rules/no-multi-spaces)`, `[react/jsx-tag-spacing](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-tag-spacing.md)`

    ```jsx
    // Bad!
    <Foo/>

    // very bad
    <Foo                 />

    // Bad!
    <Foo
     />

    // Good
    <Foo />
    ```

- Do not pad JSX curly braces with spaces. eslint: `[react/jsx-curly-spacing](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)`

    ```jsx
    // Bad!
    <Foo bar={ baz } />

    // Good
    <Foo bar={baz} />
    ```

## Props

- Always use camelCase for prop names.

    ```jsx
    // Bad!
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // Good
    <Foo
      userName="hello"
      phoneNumber={12345678}
    />
    ```

- Omit the value of the prop when it is explicitly `true`. eslint: `[react/jsx-boolean-value](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)`

    ```jsx
    // Bad!
    <Foo
      hidden={true}
    />

    // Good
    <Foo
      hidden
    />

    // Good
    <Foo hidden />
    ```

- Always include an `alt` prop on `<img>` tags. If the image is presentational, `alt` can be an empty string or the `<img>` must have `role="presentation"`. eslint: `[jsx-a11y/alt-text](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/alt-text.md)`

    ```jsx
    // Bad!
    <img src="hello.jpg" />

    // Good
    <img src="hello.jpg" alt="Me waving hello" />

    // Good
    <img src="hello.jpg" alt="" />

    // Good
    <img src="hello.jpg" role="presentation" />
    ```

- Do not use words like "image", "photo", or "picture" in `<img>` `alt` props. eslint: `[jsx-a11y/img-redundant-alt](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md)`

    > Why? Screenreaders already announce img elements as images, so there is no need to include this information in the alt text.

    ```jsx
    // Bad!
    <img src="hello.jpg" alt="Picture of me waving hello" />

    // Good
    <img src="hello.jpg" alt="Me waving hello" />
    ```

- Use only valid, non-abstract [ARIA roles](https://www.w3.org/TR/wai-aria/#usage_intro). eslint: `[jsx-a11y/aria-role](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/aria-role.md)`

    ```jsx
    // Bad! - not an ARIA role
    <div role="datepicker" />

    // Bad! - abstract ARIA role
    <div role="range" />

    // Good
    <div role="button" />
    ```

- Do not use `accessKey` on elements. eslint: `[jsx-a11y/no-access-key](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md)`

> Why? Inconsistencies between keyboard shortcuts and keyboard commands used by people using screenreaders and keyboards complicate accessibility.

```jsx
// Bad!
<div accessKey="h" />

// Good
<div />
```

- Avoid using an array index as `key` prop, prefer a stable ID. eslint: `[react/no-array-index-key](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-array-index-key.md)`

> Why? Not using a stable ID is an anti-pattern because it can negatively impact performance and cause issues with component state.

We don’t recommend using indexes for keys if the order of items may change.

```jsx
// Bad!
{todos.map((todo, index) =>
  <Todo
    {...todo}
    key={index}
  />
)}

// Good
{todos.map(todo => (
  <Todo
    {...todo}
    key={todo.id}
  />
))}
```

- Always define explicit defaultProps for all non-required props.

> Why? propTypes are a form of documentation, and providing defaultProps means the reader of your code doesn’t have to assume as much. In addition, it can mean that your code can omit certain type checks.

```jsx
// Bad!
function SFC({ foo, bar, children }) {
  return <div>{foo}{bar}{children}</div>;
}
SFC.propTypes = {
  foo: PropTypes.number.isRequired,
  bar: PropTypes.string,
  children: PropTypes.node,
};

// Good
function SFC({ foo, bar, children }) {
  return <div>{foo}{bar}{children}</div>;
}
SFC.propTypes = {
  foo: PropTypes.number.isRequired,
  bar: PropTypes.string,
  children: PropTypes.node,
};
SFC.defaultProps = {
  bar: '',
  children: null,
};
```

- Use spread props sparingly.

> Why? Otherwise you’re more likely to pass unnecessary props down to components. And for React v15.6.1 and older, you could pass invalid HTML attributes to the DOM.

Exceptions:

- HOCs that proxy down props and hoist propTypes

```jsx
function HOC(WrappedComponent) {
  return class Proxy extends React.Component {
    Proxy.propTypes = {
      text: PropTypes.string,
      isLoading: PropTypes.bool
    };

    render() {
      return <WrappedComponent {...this.props} />
    }
  }
}
```

- Spreading objects with known, explicit props. This can be particularly useful when testing React components with Mocha’s beforeEach construct.

```jsx
export default function Foo {
  const props = {
    text: '',
    isPublished: false
  }

  return (<div {...props} />);
}
```

Notes for use:
Filter out unnecessary props when possible. Also, use [prop-types-exact](https://www.npmjs.com/package/prop-types-exact) to help prevent bugs.

```jsx
// Bad!
render() {
  const { irrelevantProp, ...relevantProps } = this.props;
  return <WrappedComponent {...this.props} />
}

// Good
render() {
  const { irrelevantProp, ...relevantProps } = this.props;
  return <WrappedComponent {...relevantProps} />
}
```

## Refs

- Always use ref callbacks. eslint: `[react/no-string-refs](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-string-refs.md)`

    ```jsx
    // Bad!
    <Foo
      ref="myRef"
    />

    // Good
    <Foo
      ref={(ref) => { this.myRef = ref; }}
    />
    ```

## Parentheses

- Wrap JSX tags in parentheses when they span more than one line. eslint: `[react/jsx-wrap-multilines](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-wrap-multilines.md)`

    ```jsx
    // Bad!
    render() {
      return <MyComponent variant="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // Good
    render() {
      return (
        <MyComponent variant="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }

    // Good, when single line
    render() {
      const body = <div>hello</div>;
      return <MyComponent>{body}</MyComponent>;
    }
    ```

## Tags

- Always self-close tags that have no children. eslint: `[react/self-closing-comp](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)`

    ```jsx
    // Bad!
    <Foo variant="stuff"></Foo>

    // Good
    <Foo variant="stuff" />
    ```

- If your component has multiline properties, close its tag on a new line. eslint: `[react/jsx-closing-bracket-location](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)`

    ```jsx
    // Bad!
    <Foo
      bar="bar"
      baz="baz" />

    // Good
    <Foo
      bar="bar"
      baz="baz"
    />
    ```

## Methods

- Use arrow functions to close over local variables. It is handy when you need to pass additional data to an event handler. Although, make sure they [do not massively hurt performance](https://www.bignerdranch.com/blog/choosing-the-best-approach-for-react-event-handlers/), in particular when passed to custom components that might be PureComponents, because they will trigger a possibly needless rerender every time.

    ```jsx
    function ItemList(props) {
      return (
        <ul>
          {props.items.map((item, index) => (
            <Item
              key={item.key}
              onClick={(event) => { doSomethingWith(event, item.name, index); }}
            />
          ))}
        </ul>
      );
    }
    ```

- Bind event handlers for the render method in the constructor. eslint: `[react/jsx-no-bind](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)`

    > Why? A bind call in the render path creates a brand new function on every single render. Do not use arrow functions in class fields, because it makes them challenging to test and debug, and can negatively impact performance, and because conceptually, class fields are for data, not logic.

    ```jsx
    // Bad!
    class extends React.Component {
      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv.bind(this)} />;
      }
    }

    // very bad
    class extends React.Component {
      onClickDiv = () => {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv} />
      }
    }

    // Good
    class extends React.Component {
      constructor(props) {
        super(props);

        this.onClickDiv = this.onClickDiv.bind(this);
      }

      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv} />;
      }
    }
    ```

- Do not use underscore prefix for internal methods of a React component.

    > Why? Underscore prefixes are sometimes used as a convention in other languages to denote privacy. But, unlike those languages, there is no native support for privacy in JavaScript, everything is public. Regardless of your intentions, adding underscore prefixes to your properties does not actually make them private, and any property (underscore-prefixed or not) should be treated as being public. See issues #1024, and #490 for a more in-depth discussion.

    ```jsx
    // Bad!
    React.createClass({
      _onClickSubmit() {
        // do stuff
      },

      // other stuff
    });

    // Good
    class extends React.Component {
      onClickSubmit() {
        // do stuff
      }

      // other stuff
    }
    ```

- Be sure to return a value in your `render` methods. eslint: `[react/require-render-return](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/require-render-return.md)`

    ```jsx
    // Bad!
    render() {
      (<div />);
    }

    // Good
    render() {
      return (<div />);
    }
    ```

## Ordering

- Ordering for `class extends React.Component`:
1. optional `static` methods
2. `constructor`
3. `getChildContext`
4. `componentWillMount`
5. `componentDidMount`
6. `componentWillReceiveProps`
7. `shouldComponentUpdate`
8. `componentWillUpdate`
9. `componentDidUpdate`
10. `componentWillUnmount`
11. *clickHandlers or eventHandlers* like `onClickSubmit()` or `onChangeDescription()`
12. *getter methods for `render`* like `getSelectReason()` or `getFooterContent()`
13. *optional render methods* like `renderNavigation()` or `renderProfilePicture()`
14. `render`
- How to define `propTypes`, `defaultProps`, `contextTypes`, etc...

    ```jsx
    import React from 'react';
    import PropTypes from 'prop-types';

    const propTypes = {
      id: PropTypes.number.isRequired,
      url: PropTypes.string.isRequired,
      text: PropTypes.string,
    };

    const defaultProps = {
      text: 'Hello World',
    };

    class Link extends React.Component {
      static methodsAreOk() {
        return true;
      }

      render() {
        return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>;
      }
    }

    Link.propTypes = propTypes;
    Link.defaultProps = defaultProps;

    export default Link;
    ```

- Ordering for `React.createClass`: eslint: `[react/sort-comp](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/sort-comp.md)`
1. `displayName`
2. `propTypes`
3. `contextTypes`
4. `childContextTypes`
5. `mixins`
6. `statics`
7. `defaultProps`
8. `getDefaultProps`
9. `getInitialState`
10. `getChildContext`
11. `componentWillMount`
12. `componentDidMount`
13. `componentWillReceiveProps`
14. `shouldComponentUpdate`
15. `componentWillUpdate`
16. `componentDidUpdate`
17. `componentWillUnmount`
18. *clickHandlers or eventHandlers* like `onClickSubmit()` or `onChangeDescription()`
19. *getter methods for `render`* like `getSelectReason()` or `getFooterContent()`
20. *optional render methods* like `renderNavigation()` or `renderProfilePicture()`
21. `render`

## `isMounted`

- Do not use `isMounted`. eslint: `[react/no-is-mounted](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)`

> Why? isMounted is an anti-pattern, is not available when using ES6 classes, and is on its way to being officially deprecated.

## Folder Structures 📂

```
|── public
│   ├── css
│   ├── favicons
│   ├── icons
│   ├── img
│   │   ├── icons
│   │   └── mocks
│   ├── browserconfig.xml
│   ├── index.html
│   ├── manifest.json
│   ├── manifest.webapp
│   ├── robots.txt
│   └── worker.js
├── src
│   ├── components
│   │   ├── Component
│   │   │   |── _style.scss
│   │   │   └── index.js
│   ├── db
│   │   └── mock.js
│   ├── layout
│   │   ├── _style.scss
│   │   ├── index.js
│   │   ├── reducer.js
│   │   ├── saga.js
│   │   └── view.js
│   ├── pages
│   │   ├── page
│   │   │   ├── _style.scss
│   │   │   ├── index.js
│   │   │   ├── reducer.js
│   │   │   ├── saga.js
│   │   │   └── view.js
│   ├── widgets
│   │   ├── Widget
│   │   │   ├── _style.scss
│   │   │   ├── index.js
│   │   │   ├── reducer.js
│   │   │   ├── saga.js
│   │   │   └── view.js
│   ├── redux
│   │   ├── reducers.js
│   │   ├── sagas.js
│   │   └── store.js
│   ├── routes
│   │   └── index.js
│   ├── styles
│   │   ├── global.scss
│   │   ├── icons.scss
│   │   ├── index.scss
│   │   ├── mixins.scss
│   │   ├── reset.scss
│   │   └── vars.scss
│   ├── tests
│   │   ├── config.actions.test.js
│   │   └── reducers.test.js
│   ├── utils
│   │   ├── config.actions.js
│   │   ├── config.axios.js
│   │   ├── config.context.js
│   │   ├── config.firebase.js
│   │   ├── config.handlers.js
│   │   ├── config.helpers.js
│   │   ├── config.saga.js
│   │   ├── config.sw.js
│   │   └── config.ui.js
│   └── widgets
│   │   ├── config.actions.js
│   │   ├── config.axios.js
│   │   ├── config.handlers.js
│   │   ├── config.localization.js
│   │   ├── config.saga-crud.js
│   │   ├── config.context.js
│   │   ├── config.saga.js
│   │   └── config.ui.js
│   └── index.js
├── .babelrc
├── .codeclimate.yml
├── .dockerignore
├── .editorconfig
├── .eslintrc
├── .gitignore
├── .gitlab-ci.yml
├── .prettierignore
├── .prettierrc
├── config-overrides.js
├── Dockerfile
├── jsconfig.json
├── package-lock.json
├── package.json
├── README.md
├── server.js
├── STYLEGUIDE.md
└── TODO.md
```
