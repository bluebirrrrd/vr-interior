# vr-interior
Web app for viewing your 3D models (mostly for interior design). Built upon <b><a href="https://github.com/prayasht/aframe-preact-starter">aframe-preact-starter</a><b>.
<br/>

<h5 align="center">
  Build virtual reality experiences with <b><a href="https://aframe.io">A-Frame</a></b> and <b><a href="https://preactjs.com/">Preact</a></b>.
</h5>

## Getting Started

* Install (if you don't have them):
    * [Node.js](http://nodejs.org): `brew install node` on OS X
    * [Brunch](http://brunch.io): `npm install -g brunch`
    * Brunch plugins and app dependencies: `npm install`
* Run:
    * `npm start` — watches the project with continuous rebuild. This will also launch HTTP server with [pushState](https://developer.mozilla.org/en-US/docs/Web/Guide/API/DOM/Manipulating_the_browser_history).
    * `npm run build` — builds minified project for production
* Learn:
    * `public/` dir is fully auto-generated and served by HTTP server.  Write your code in `app/` dir.
    * Place static files you want to be copied from `app/assets/` to `public/`.
    * [Brunch site](http://brunch.io), [Getting started guide](https://github.com/brunch/brunch-guide#readme)

Install with [npm](https://www.npmjs.com/package/aframe-react) or
[yarn](https://github.com/yarnpkg/yarn).

```
npm install --save aframe aframe-react react react-dom
yarn add --save aframe aframe-react react react-dom
```


## Example

```javascript
import 'aframe'
import { h, Component, render } from 'preact'
import { Entity, Scene } from 'aframe-react'
import 'aframe-particle-system-component'
import 'aframe-animation-component'

class App extends React.Component {
  render () {
    return (
      <Scene>
        <Entity geometry={{primitive: 'box'}} material={{color: 'red'}} position={{x: 0, y: 0, z: -5}}/>
        <Entity particle-system={{preset: 'snow'}}/>
        <Entity light={{type: 'point'}}/>
        <Entity gltf-model={{src: 'virtualcity.gltf'}}/>
        <Entity text={{value: 'Hello, WebVR!'}}/>
      </Scene>
    );
  }
}

document.addEventListener('DOMContentLoaded', () => {
  render(<App />, document.querySelector('#app'))
})
```

The following has been extracted from the [aframe-react](https://github.com/aframevr/aframe-react) page, but the same concepts still apply to Preact.

## Introduction

![A-Frame](https://cloud.githubusercontent.com/assets/674727/24384472/e833ccee-1318-11e7-81a5-61e782f5b472.png)

[A-Frame](https://aframe.io) is a web framework for building virtual reality
experiences. Since A-Frame is built on top of the DOM, web libraries such as
React, Vue.js, Angular, Ember.js, d3.js are able to sit cleanly on top of
A-Frame.

A-Frame is an [entity-component-system (ECS) framework exposed through
HTML](https://aframe.io/docs/). ECS is a pattern used in game development that
favors composability over inheritance, which is more naturally suited to 3D
scenes where objects are built of complex appearance, behavior, and
functionality. In A-Frame, HTML attributes map to *components* which are
composable modules that are plugged into `<a-entity>`s to attach appearance,
behavior, and functionality.

Released on the same day as A-Frame, `aframe-react` is a very thin layer on top
of A-Frame to bridge with React. `aframe-react` passes React props to directly
A-Frame using refs and `.setAttribute()`, bypassing the DOM. This works since
A-Frame's `.setAttribute()`s are able to take non-string data such as objects,
arrays, or elements and synchronously modify underlying 3D scene graph.

```js
// aframe-react's <Entity/> React Component
<Entity geometry={{primitive: 'box', width: 5}} position="0 0 -5"/>

// renders
<a-entity>

// and then applies the data directly to the underlying 3D scene graph, bypassing the DOM.
<a-entity>.setAttribute('geometry', {primitive: 'box', width: 5});
<a-entity>.setAttribute('position', '0 0 -5');
```

`aframe-react` provides the best of both worlds between A-Frame and React, the
3D and VR-oriented entity-component architecture of A-Frame, and the view and
state management ergonomics of React, without penalties of attempting to use
React for a VR application.

## Making React Viable

A-Frame and `aframe-react` gives some viability for React to use in VR
applications. The value proposition of React is limited to the 2D Web:

- Improve rendering performance for 2D web pages by reducing calls to the
  browser's 2D layout engine via the virtual DOM.
- Provide an predictable application structure for large 2D web applications through
  a structured nesting of React components, data binding, and one-way data flow.

However, these propositions fall short in 3D and VR applications:

- In 3D and VR applications, including A-Frame, the 2D browser layout engine is
  not touched. Thus React's reconciliation engine can become an unpredictable
  performance liability for little benefit.
- In 3D and VR applications, objects are not structured in a top-down hierarchy like
  2D web applications. 3D objects can exist anywhere and interact with any other 3D object
  in a many-to-many manner. But React's paradigm prescribes data flow from parent-to-child,
  which makes it restrictive for 3D and VR applications.

A-Frame and aframe-react gives meaning and purpose to React: **A-Frame provides
an actual DOM for React to reconcile, diff, and bind to**.

React also raises questions around performance; React is a very large library
designed for a 60fps 2D Web, is it equipped to handle 90fps VR applications?
90 prop updates per second per object for many objects through React's engine
causes concern in the overhead added for each operation.

aframe-react lets A-Frame handle the heavy lifting 3D and VR rendering and
behavior. A-Frame is optimized from the ground up for WebVR with a 3D-oriented
entity-component architecture. And **aframe-react lets React focus on what it's
good at: views, state management, and data binding.**

### Entity-Component Meets React

A-Frame's entity-component-system (ECS) pattern is tailored for 3D and VR
applications. What React did for 2D web applications is what ECS did for 3D
applications in terms of providing a useful abstraction. ECS promotes
composability over hierarchy. In 3D applications, composability refers to
composition of appearance, behavior, and logic rather than having fixed blocks.
This lets us do things like define a flying robot that explodes on contact and
makes robot sounds by snapping together a pre-existing model component, explode
component, event handler component, sound component, and flying component.

Unfortunately, React (and the 2D web for that matter) is heavily hierarchical,
which is not suited for 3D. Whereas 2D web development consisted of structuring
and laying out from an assorted set of fixed 2D elements (e.g., `<p>`, `<a>`,
`<img>`, `<form>`), 3D development involves objects that are infinite in
complexity and type. ECS provides an easy way of defining those objects by
mixing and matching plug-and-play components.

With aframe-react, we get the best of both worlds. The 3D and VR architecture
of A-Frame, and the view and state ergonomics of React. React can be used to
bind application and state data to the values of A-Frame components. And we
still have access to all the features and performance of A-Frame as well as
[A-Frame's community component ecosystem](https://aframe.io/registry/).

## API

`aframe-react`'s API is very thin on top of A-Frame, less than 200 lines of
source code. The API consists of just two React Components: `<Entity/>` and
`<Scene/>`.

#### \<Entity {...components}/>

`<Entity/>` wraps
[`<a-entity>`](https://aframe.io/docs/0.5.0/core/entity.html), the *entity*
piece of the [entity-component-system
pattern](https://aframe.io/docs/0.5.0/core/). Plug in [A-Frame
components](https://aframe.io/docs/0.5.0/introduction/#entity-component-system)
as React props to attach appearance, behavior, or functionality to the
`<Entity/>`.

```jsx
<Scene>
  <Entity
    geometry={{primitive: 'box', width: 5}}
    material={{color: red, roughness: 0.5, src: texture.png}}
    scale={{x: 2, y: 2, z: 2}}
    position={{x: 0, y: 0, z: -5}}/>
</Scene>
```

[Community A-Frame components](https://aframe.io/registry/) can be imported and
installed through npm:

```
import 'aframe-particle-system-component';
import 'aframe-mountain-component';

// ...

<Scene>
  <Entity mountain/>
  <Entity particle-system={{preset: 'snow', particleCount: 5000}}/>
</Scene>
```

#### `primitive`

To use A-Frame [primitives](https://aframe.io/docs/0.5.0/primitives/), provide
the `primitive` prop with the primitive's element name (e.g., `a-sphere`).
Mappings can be applied the same as in HTML through React props:

```jsx
<Entity primitive='a-box' color="red" position="0 0 -5"/>
<Entity primitive='a-sphere' color="green" position="-2 0 -3"/>
<Entity primitive='a-cylinder' color="blue" position="2 0 -3"/>
<Entity primitive='a-sky' src="sechelt.jpg"/>
```

#### `events`

To register event handlers, use the `events` prop. `events` takes a mapping of
event names to event handler(s). Multiple event handlers can be provided for a
single event name by providing an array of functions. Try not to pass in inline
functions to not trigger unnecessary React renders. Pass in binded functions
instead.

For example, using the synthetic `click` event provided by A-Frame's `cursor`
component, or a `collided` event possibly provided by a physics component.

```jsx
handleClick = () => {
  console.log('Clicked!');
}

handleCollide = () => {
  console.log('Collided!');
}

render() {
  return (
    <Scene>
      <Entity events={{
        click: this.handleClick,
        collided: [this.handleCollide]}}/>
    </Scene>
  );
}
```

`aframe-react` does not support React-style `onXXX` event handlers (e.g.,
`onClick`). Unlike 2D web pages, VR sites are composed entirely of custom
synthetic event names (which could have hyphens, be all lowercase, be
camelCase, all uppercase, camel case, etc.,). The possible event names are
infinite. The `events` prop makes it explicit what the event names to handle
are.

#### `_ref`

Use `aframe-react`'s `_ref` prop to add a callback to the underlying
`<a-entity>` DOM node:

```
<Entity _ref={this.entityCallback}/>
```

#### \<Scene {...components}/>

`<Scene/>` extends `<Entity/>` and renders `<a-scene>` instead of `<a-entity>`.
Place all `<Entity/>`s as a child of the `<Scene/>` Component. There should
only be one `<Scene/>` per page:

```jsx
<Scene>
  <Entity/>
</Scene>
```

## Best Practices

Let A-Frame and three.js handle the heavy lifting of 3D rendering and behavior,
and delegate React to what it was meant for: views, state management, and data
binding.

For example, don't create `requestAnimationFrame`s to continuously throw React
prop updates to A-Frame. Instead, create an A-Frame component with a tick
handler such that everything is done in memory with little overhead. Do however
use React to set up initial state and data binding that might configure that
A-Frame component.

## Using with Preact

`aframe-react` works with Preact. Since `aframe-react` uses `React.Component`,
we have to tell Webpack to alias that to `Preact.Component`:

```
resolve: {
  alias: {
    react: 'preact'
  }
}
```
