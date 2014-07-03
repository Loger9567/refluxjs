# Reflux

A simple library for uni-directional dataflow architecture inspired by ReactJS [Flux](http://facebook.github.io/react/blog/2014/05/06/flux.html).

You can read an overview of Flux [here](http://facebook.github.io/react/docs/flux-overview.html), however the gist of it is to introduce a more functional programming style architecture by eschewing MVC like pattern and adopting a single data flow pattern. The pattern is composed of dispatching actions and data stores. Every data manipulating event needs to pass through the actions that will then appropriately dispatch the event among the data stores that listen to relevant actions.

The goal of the project is to get this architecture easily up and running in your web application, both client-side or server-side. And there are some differences between how this project works and how Facebook's variant works:

* Every action is a listenable event handler, essentially incorporating the singleton dispatcher inside each one of them
* No more type checking with strings! Since data stores know what actions they listen to, they should do just that and let the actions dispatch accordingly.
* Data stores are also listenable event handlers, and you may build an aggregate data store by listening to changes made by other data stores.

## Installation

You can currently install the package as a npm package or bower.

### NPM

The following command installs reflux as an npm package:

    npm install reflux

### Bower

The following command installs reflux as a bower component that can be used in the browser:

    bower install reflux

It will also download lodash as a dependency.

## Usage

Create an action by calling `createAction`.

```javascript
var Reflux = require('reflux');

var statusUpdate = Reflux.createAction();
```

Create a data store by passing a definition object to `createStore`.

```javascript
// Creates a DataStore
var statusStore = Reflux.createStore({

    // Initial setup
    init: function() {

        // Register statusUpdate action
        this.listenTo(statusUpdate, this.output);
    },

    // Callback
    output: function(flag) {
        var status = flag ? 'ONLINE' : 'OFFLINE';
        this.trigger(status);
    }

});
```

In your component, register to listen to your data store like this:

```javascript
// Fairly simple view component that outputs to console
function ConsoleComponent() {

    // Registers a console logging callback to the statusStore updates
    statusStore.listen(function(status) {
        console.log('status: ', status);
    });
};

var consoleComponent = new ConsoleComponent();
```

Invoke actions as if they were functions:

```javascript
statusUpdate(true);
statusUpdate(false);

/**
 * Will output:
 * status:  ONLINE
 * status:  OFFLINE
 */
```

See `test/index.js` for more example on how to use the package.

## License

This project is licensed under [BSD 3-Clause License](http://opensource.org/licenses/BSD-3-Clause). Copyright (c) 2014, Mikael Brassman. For more information [read the LICENSE.txt file](LICENSE.txt).