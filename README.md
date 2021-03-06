fps-emitter
===========

Measures the FPS (frames per second) on the current page and emits the 
result whenever it changes, as an [EventEmitter][]. Designed to be run in the browser.

Note that it only measures FPS per `requestAnimationFrame()`. Therefore it's not an accurate measure of "true" framerate (e.g. where independent composition/rendering may be involved). However it serves as a pretty good measure of how much different UI-blocking events (such as JavaScript) may be impacting your performance. For instance, it's a good measure for JavaScript animations.

Note also that the FPS measurement is rounded to the nearest integer, clamped between 0 and 60.

The library is 2.7kB minified+gzipped, or less if you're already using EventEmitters elsewhere in your code.

Install
-------

Via npm:

    npm install fps-emitter

Or via unpkg:

```html
<script src="https://unpkg.com/fps-emitter/dist/fps-emitter.min.js"></script>
```

Usage
-----

```js
var FpsEmitter = require('fps-emitter') // or window.FpsEmitter if using dist/
var fps = new FpsEmitter()

// Get the current FPS, as an integer between 0 and 60:
var currentFps = fps.get()

// Or get notified whenever it changes:
fps.on('update', function (newFps) {
   console.log('FPS is: ', newFps)
})
```

### Update interval

By default, samples are collected every 1000 milliseconds. You can change this
either in the constructor or via a runtime API:

```js
var fps = new FpsEmitter(2000); // Update every 2000 milliseconds, from the start

fps.setUpdateInterval(2000); // Change the update interval at runtime
```

### EventEmitter

The `FpsEmitter` object is an [EventEmitter][] 
that only emits one event, `'update'`. 
Standard idioms like `on()`, `.once()`, and `removeListener()` all apply.

### Debug vs production mode

Once you call the constructor (`new FpsEmitter()`), it starts tracking the global FPS using
`requestAnimationFrame()`. Simply measuring the FPS has the potential to cause slowdowns, so
you may want to disable it in production:

```js
if (DEBUG_MODE) {
  var fps = new FpsEmitter()
  // etc.
} else {
  // do nothing
}
```

Testing
-------

    npm install
    npm test

## Code of Conduct
This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

[EventEmitter]: https://nodejs.org/api/events.html#events_class_eventemitter
