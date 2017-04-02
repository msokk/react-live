<p align="center"><img src="https://raw.githubusercontent.com/philpl/react-live/master/docs/logo.gif" width=250></p>
<h2 align="center">React Live</h2>
<p align="center">
<strong>A production-focused playground for live editing React code</strong>
<br><br>
<a href="https://npmjs.com/package/react-live"><img src="https://img.shields.io/npm/dm/react-live.svg"></a>
<a href="https://npmjs.com/package/react-live"><img src="https://img.shields.io/npm/v/react-live.svg"></a>
<img src="http://img.badgesize.io/https://unpkg.com/react-live/dist/react-live.min.js?compression=gzip&label=gzip%20size">
<img src="http://img.badgesize.io/https://unpkg.com/react-live/dist/react-live.min.js?label=size">
<img src="https://img.shields.io/badge/module%20formats-umd%2C%20cjs%2C%20esm-green.svg">
</p>

**React Live** brings you the ability to render React components and present the user with editable
source code and live preview.
It supports server-side rendering and comes in a tiny bundle, thanks to Bublé and a Prism.js-based editor.

The library is structured modularly and lets you style its components as you wish and put them where you want.

## Usage

Install it with `npm install react-live` and try out this piece of JSX:

```js
import {
  LiveProvider,
  LiveEditor,
  LiveError,
  LivePreview
} from 'react-live'

<LiveProvider code="<strong>Hello World!</strong>">
  <LiveEditor />
  <LiveError />
  <LivePreview />
</LiveProvider>
```

## Demo

[https://react-live-demo.philpl.com/](https://react-live-demo.philpl.com/)

## FAQ

### How does it work?

It takes your code and transpiles it through Bublé, while the code is displayed using Prism.js.
The transpiled code is then rendered in the preview component, which does a fake mount, if the code
is a component.

Easy peasy!

### What code can I use?

The code can be one of the following things:

- React elements, e.g. `<strong>Hello World!</strong>`
- React pure functional components, e.g. `() => <strong>Hello World!</strong>`
- React component classes

It cannot contain inline code just yet.

### How does the scope work?

The `scope` prop on the `LiveProvider` accepts additional globals. By default it injects `React` only, which
means that the user can use it in his code like this:

```js
//                    ↓↓↓↓↓
class Example extends React.Component {
  render() {
    return <strong>Hello World!</strong>
  }
}
```

But you can of course pass more things to this scope, that will be available as variables in the code.

## API

### &lt;LiveProvider /&gt;

This component provides the `context` for all the other ones. It also transpiles the user's code!
It supports these props, while passing all others through to a `<div />`:

|Name|PropType|Description|
|---|---|---|
|code|PropTypes.string|The code that should be rendered, apart from the user's edits
|scope|PropTypes.object|Accepts custom globals that the `code` can use

Apart from these props it attaches the `.react-live` CSS class to its `div`.
All subsequent components must be rendered inside a provider, since they communicate
using one.

### &lt;LiveEditor /&gt;

This component renders the editor that displays the code. It is built using Prism.js and a Content Editable.
It accepts these props for styling:

|Name|PropType|Description|
|---|---|---|
|className|PropTypes.string|An additional class that is added to the Content Editable
|style|PropTypes.object|Additional styles for the Content Editable

This component renders a Prism.js editor underneath it and also renders all of Prism's
styles inside a `style` tag.
The editor / content editable has an additional `.react-live-editor` CSS class.

### &lt;LiveError /&gt;

This component renders any error that occur while executing the code, or transpiling it.
It passes through any props to its `div` and also attaches the `.react-live-error` CSS class to it.

> Note: Right now the component unmounts, when there's no error to be shown.

### &lt;LivePreview /&gt;

This component renders the actual component, that the code generates, inside an error boundary.
It passes through any props to its `div` and also attaches the `.react-live-preview` CSS class to it.

## Prior work & Thanks

This project wouldn't exist without Formidable's [component-playground](https://github.com/FormidableLabs/component-playground)
which is the actual idea behind this project as well.

So thanks to [Formidable Labs](https://formidable.com/open-source/component-playground/)!
