# Babel Preset JSX

## Installation

````js
Install the preset with:

npm install @vue/babel-preset-jsx @vue/babel-helper-vue-jsx-merge-props
Then add the preset to .babelrc:

{
  "presets": ["@vue/babel-preset-jsx"]
}
````

## Syntax

### Content

````js
render() {
  return <p>hello</p>
}
````

with dynamic content:

````js
render() {
  return <p>hello { this.message }</p>
}
````

when self-closing:

````js
render() {
  return <input />
}
````

with a component:

````js
import MyComponent from './my-component'

export default {
  render() {
    return <MyComponent>hello</MyComponent>
  },
}
````

### Attributes/Props

````js
render() {
  return <input type="email" />
}
````

with a dynamic binding:

````js
render() {
  return <input
    type="email"
    placeholder={this.placeholderText}
  />
}
````

with the spread operator (object needs to be compatible with Vue Data Object):

````js
render() {
  const inputAttrs = {
    type: 'email',
    placeholder: 'Enter your email'
  }

  return <input {...{ attrs: inputAttrs }} />
}
````

### Slots

named slots:
````js
render() {
  return (
    <MyComponent>
      <header slot="header">header</header>
      <footer slot="footer">footer</footer>
    </MyComponent>
  )
}
````

scoped slots:
````js
render() {
  const scopedSlots = {
    header: () => <header>header</header>,
    footer: () => <footer>footer</footer>
  }

  return <MyComponent scopedSlots={scopedSlots} />
}
````

### Directives

````js
<input vModel={this.newTodoText} />
````

with a modifier:

````js
<input vModel_trim={this.newTodoText} />
````

with an argument:

````js
<input vOn:click={this.newTodoText} />
````

with an argument and modifiers:

````js
<input vOn:click_stop_prevent={this.newTodoText} />
````

v-html:

````js
<p domPropsInnerHTML={html} />
````

### Functional Components

Transpiles arrow functions that return JSX into functional components, when they are either default exports:

````js
export default ({ props }) => <p>hello {props.message}</p>
````

or PascalCase variable declarations:

````js
const HelloWorld = ({ props }) => <p>hello {props.message}</p>
````

## Compatibility

This repo is only compatible with:

* Babel 7+. For Babel 6 support, use vuejs/babel-plugin-transform-vue-jsx
* Vue 2+. JSX is not supported for older versions.