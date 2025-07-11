# Dataroom.js

`Dataroom.js` provides a powerful and flexible custom element, `DataroomElement`, that serves as a foundation for creating interactive data visualization components. This document details its features and provides examples of how to extend and utilize them.

## Installation

```bash
npm install @dataroom/js
```

## Basic Usage

To use `DataroomElement`, you can either use it directly in your HTML or extend it to create your own custom components.

### Extending `DataroomElement`

```javascript
import DataroomElement from '@dataroom/js';

class MyComponent extends DataroomElement {
  async initialize() {
    // Your initialization logic here
  }
}

customElements.define('my-component', MyComponent);
```

## Features

### `create(type, attributes, target_el)`

Creates a new HTML element and appends it to the component or a specified target.

**Example:**

```javascript
class MyComponent extends DataroomElement {
  async initialize() {
    const container = this.create('div', { class: 'container' });
    this.create('p', { content: 'Hello, World!' }, container);
  }
}
```

### `event(name, detail)`

Emits a custom event from the component.

**Example:**

```javascript
class MyComponent extends DataroomElement {
  async initialize() {
    this.event('my-event', { foo: 'bar' });
  }
}

const myComponent = document.querySelector('my-component');
myComponent.addEventListener('my-event', (e) => {
  console.log(e.detail); // { foo: 'bar' }
});
```

### `on(name, cb)`

Attaches an event listener to the component.

**Example:**

```javascript
class MyComponent extends DataroomElement {
  async initialize() {
    this.on('my-event', (detail) => {
      console.log(detail); // { foo: 'bar' }
    });

    this.event('my-event', { foo: 'bar' });
  }
}
```

### `call(endpoint, body)`

A helper for making `fetch` requests. It includes features for handling different security schemes and request timeouts.

#### `security-scheme` Attribute

Determines the authentication method:

*   `localstorage`: (Default) Sends a bearer token from `localStorage`.
*   `cookie`: Relies on the browser to send cookies automatically.

**Example:**

```html
<my-component security-scheme="localstorage"></my-component>
```

```javascript
// In your component
const data = await this.call('/api/data');
```

#### `call-timeout` Attribute

Sets a timeout for the request in milliseconds.

**Example:**

```html
<my-component call-timeout="5000"></my-component>
```

### `log(message)`

Logs a message to the console if the `verbose` attribute is set.

**Example:**

```html
<my-component verbose="true"></my-component>
```

```javascript
class MyComponent extends DataroomElement {
  async initialize() {
    this.log('Initializing component...');
  }
}
```

### Lifecycle Methods

`DataroomElement` provides several lifecycle methods that you can override to control the component's behavior.

*   `initialize()`: Called after the component is connected to the DOM and its attributes are available.
*   `disconnect()`: Called when the component is removed from the DOM.

**Example:**

```javascript
class MyComponent extends DataroomElement {
  async initialize() {
    console.log('Component initialized!');
  }

  async disconnect() {
    console.log('Component disconnected!');
  }
}
```

### Attribute Observation

`DataroomElement` automatically observes attribute changes and fires a `NODE-CHANGED` event.

**Example:**

```javascript
class MyComponent extends DataroomElement {
  async initialize() {
    this.on('NODE-CHANGED', (detail) => {
      console.log(detail.attribute, detail.newValue);
    });
  }
}

const myComponent = document.querySelector('my-component');
myComponent.setAttribute('foo', 'bar'); // Logs: foo bar
```