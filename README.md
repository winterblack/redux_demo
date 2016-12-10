# Redux Demo

## Stage 1 - Node Package Manager and Webpack

`npm init --yes`

`npm install --save babel-core babel-loader babel-preset-es2015 babel-preset-react lodash react react-dom react-redux redux webpack`

set up `webpack.config.js`

## Stage 2 - Set Up your Entry File and Index HTML

create `entry.jsx`

* import React and ReactDOM
* add event listener on dom content loaded
* create React component to render

create `index.html`

* add a script src tag for your bundle.js
* add an index div

Time to start using `webpack --watch`. Leave it on whenever you're working on your project.

You should be able to see your react component now.

## Stage 3 - Create your Actions, Reducers, and Store

`touch actions/some_actions.js`

* export switch case constants
* export action creators

`touch reducers/some_reducer.js`

* import switch case constants
* import merge from 'lodash/merge'
* set defaultState constant
* export YourReducer

create `store.js`

* `import { createStore } from 'redux'`
* import YourReducer
* export Store

To test this stage, set window.store to your store. You can test store.dispatch() and store.getState()

## Stage 4 - Create your React Components

`mkdir components/some_component`

`touch components/some_component/some_component`
(You'll see why in the next stage)

There are two types of React components in ES6. Functional Components, and Components that inherit from the React.Component class. Functional components just return html, while class Components can handle their own internal state. If your component doesn't need an internal state that's different from your store's state, you should use a functional component.

A functional component looks like this:

    const SomeComponent = () => (
      <div>Your Component</div>
    )

A class component looks like this:

    class SomeComponent extends React.Component {
      constructor(props) {
        super(props)
        this.state = {}
      }

      render() {
        return (
          <div>Your Component</div>
        )
      }
    }

Export your component and import it in your entry file so you can see it on your web page. If you have a lot of components you could break your index component out into a separate file and just import that into your entry file.

## Stage 5 - Connecting your Components to your Store

This is the reason to use Redux. It's so cool!

The first step is to use Provider to add your store to all of your components' props.

In your Index component:

    import { Provider } from 'react-redux'

    const Index = ({ store }) => (
      <Provider store={ store }>
        <App/>
      </Provider>
    )

`touch components/some_component/some_component_connect`

* `import { connect } from 'react-redux'`
* import SomeComponent
* import your actions

In your component_connect:

    const mapStateToProps = state => ({
      prop: state.prop
    })

    const mapDispatchToProps = dispatch => ({
      action: () => dispatch(action())
    })

    export default connect(
      mapStateToProps, mapDispatchToProps)(SomeComponent)

`mapStateToProps` means that your components' props will always be updated whenever your store is updated. You don't have to attach any listeners to your store, just access the store directly through your props. Amazing!

`mapDispatchToProps` means that you can dispatch actions through your props. You might have a button that looks like this:

    <button onClick={props.action}>Action</button>

Amazing!

## One last thing - Combine Reducers

At some point you might have more than one reducer, and you'll want to combine them, because your store only takes one reducer.

`touch reducers/combine_reducers.js`

* `import { combineReducers } from 'redux'`
* import your reducers
* `export default combineReducers({ your, reducers })`

Then just make sure that you import your `combine_reducers.js` into your `store.js`

That's it, done, easy!
