# Babel Plugin: Annotate React
[![CircleCI](https://circleci.com/gh/freshpaint-io/freshpaint-babel-plugin-annotate-react.svg?style=svg)](https://circleci.com/gh/freshpaint-io/freshpaint-babel-plugin-annotate-react)

This is a Babel plugin that annotates React components with stable attributes that can be used to select using Freshpaint. This is most useful when using a React system that generates dynamic names for Components or rearranges elements.

## Installation
You can install via npm with `npm install --save @freshpaint/babel-plugin-annotate-react` 
or with yarn `yarn add @freshpaint/babel-plugin-annotate-react`

After that, you will need to add the plugin to your babel config.
```json
{
  "plugins": [
      ... your other plugins ...
      "@freshpaint/babel-plugin-annotate-react"
  ]
}
```

## Web
For React on the web, this library will attach attributes: `data-component`, `data-element`, and `data-source-file` to your rendered html components.

Example input:

    class HelloComponent extends Component {
      render() {
        return <div>
          <h1>Hello world</h1>
        </div>;
      }
    }

Example JS output:

    class HelloComponent extends Component {
      render() {
        return React.createElement("div", {
          "data-component": "HelloComponent",
          "data-file-source": "hello-component.js"
        }, React.createElement("h1", {
          null
        }, "Hello world"));
      }
    }

Final render:

    <div data-component="HelloComponent" data-file-source="hello-component.js">
      <h1>Hello world</h1>
    </div>

### Fragments
By default, the plugin does not annotate `React.Fragment`s because they may or may not contain a child that ends up being an HTML element.

An example with no child element:

    const componentName = () => (
      <Fragment>Hello, there.</Fragment>
    );

An example with child elements:

    const componentName = () => (
      <Fragment>
        Some text
        <h1>Hello, there.</h1> /* This one could be annotated */
        <a href="#foo">Click me</a>
      </Fragment>
    );


If you would like the plugin to attempt to annotate the first HTML element created by a Fragment (if it exists) then set the `annotate-fragments` flag:

    plugins: [
      ["@freshpaint/babel-plugin-annotate-react", { "annotate-fragments": true }]
    ]


## React Native
For React Native, this library will attach attributes: `dataComponent`, `dataElement`, and `dataSourceFile` to your components.

To activate React Native support you must pass in a `native` plugin option like so:

    plugins: [
      ["@freshpaint/babel-plugin-annotate-react", { native: true }]
    ]

## Demos
We have a few samples to demonstrate this plugin:

- [Single Page App](./samples/single-page-app/)
- [styled-components](./samples/styled-components/)
- [React native](./samples/react-native-app/)

Much of the logic for adding the attributes originated in the [transform-react-qa-classes](https://github.com/davesnx/babel-plugin-transform-react-qa-classes/) plugin.
