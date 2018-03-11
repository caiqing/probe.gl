# Log

A simple console wrapper with a host of features
* Safely exposes advanced features of Chrome and Firefox console APIs, as well as Node.js color logging, by providing fallbacks for missing methods in other environments.
* Conditional logging - includes a log levels system (aka priorities) that can be controller in the browser console, and settings persist through browser reloads.
* Defeats log cascades - Caches warnings to ensure only one instance of each warning is emitted
* Links to log calls in browser console - Clicking on a log message shows the code that called the log function.
* Image logging - In Chrome console, it is possible log images
* Improved `assert` messages - Reformats errors from `assert` to show actual error string


## Usage

Create a new Log
```js
import {Log} from 'probe.gl';
const log = new Log({id: 'my-app'});
log.log('Hello world')();  // <<< Note: double function call, is necessary
```

Add color (only affects output in Node.js)
```js
import {Log, COLOR} from 'probe.gl';
...
log.log({message: 'Hello world', color: COLOR.GREEN});
```

Log using a message generating function, rather than string (avoid creating message when not needed)
```js
log.log(2, () => `${expensiveFunction()}`)();
```

## Types and Parameters

### Log Function Parameters

Log functions can be called with different parameter combinations
- `log.log(message, ...args)` - priority defaults to 0
- `log.log(priority, message, ...args)` - sets priority
- `log.log({message, ...options})` - additional options can be set, priority is zero
- `log.log({priority, message, args, ...options})` - additional options can be set


### Log Function Options

When using named parameters (passing an object as first parameter), the following options can be used:

| Option     | Type     | Description |
| ---        | ---      | ---         |
| `priority` | `Number` | This probe will only "fire" if the log's current priority is greater than or equal to this value.defaults to `0`, which means that the probe is executed / printed regardless of log level. |
| `color`    | `enum`   | Basic colors like `green`, `blue` and `red` are supported, currently only for console logging. `import {COLOR} from 'probe.gl'` |


### Log Messages

The `message` argument can be a string or a function returning a string.


### Colors

To get access to color definitions:
`import {COLOR} from 'probe.gl'`

Available colors:
* `COLOR.BLACK`, `COLOR.RED`, `COLOR.GREEN`, `COLOR.YELLOW`, `COLOR.BLUE`, `COLOR.MAGENTA`, `COLOR.CYAN`, `COLOR.WHITE`
* `COLOR.BRIGHT_BLACK`, `COLOR.BRIGHT_RED`, `COLOR.BRIGHT_GREEN`, `COLOR.BRIGHT_YELLOW`, `COLOR.BRIGHT_BLUE`, `COLOR.BRIGHT_MAGENTA`, `COLOR.BRIGHT_CYAN`, `COLOR.BRIGHT_WHITE`


## Methods

### constructor

Creates a new `Log` instance.

`new Log({id})`


### getPriority

`log.getPriority()`


### log

Log a debug level message (uses `console.debug` if available)

`log.log(message, ...args)`
`log.log(priority, message, ...args)`
`log.log({priority, message, args, ....options})`

Returns: a function closure that should be called immediately.


### info

Log a normal message (uses `console.info` if available)

`log.info(message, ...args)`
`log.info(priority, message, ...args)`
`log.info({priority, message, args, ....options})`

Returns: a function closure that should be called immediately.


### once

Log a normal message, but only once, no console flooding

`once(priority|opts, arg, ...args)`

Returns: a function closure that should be called immediately.


### warn

Logs a warning (uses the `console.warn` method if available). Logs each warning only once to avoid console flooding.

`log.warn(message, ...args)`
`log.warn({message, args, ....options})`

Returns: a function closure that should be called immediately.


### error

Print an error, using the console's error method

`log.error(message, ...args)`
`log.error({message, args, ....options})`

Returns: a function closure that should be called immediately.


### deprecated

Generates a deprecation warning (using `log.warn`):
"`oldUsage` is deprecated and will be removed in a later version. Use `newUsage` instead."

`log.deprecated(oldUsage, newUsage)`
* `oldUsage` - name of deprecated function or parameter
* `newUsage` - name of new function or parameter

Returns: a function closure that should be called immediately.


### table

Logs a table (using `console.table` if available).

`log.table(priority|opts, table)`

Returns: a function closure that should be called immediately.


### image

Logs an image (under Chrome)

`log.image({priority, image, message = '', scale = 1})`


### time

`log.time(priority, label)`


### timeEnd

`log.timeEnd(priority, label)`


### group

`log.group(priority, arg, {collapsed = false} = {})`


### groupEnd

`log.groupEnd(priority, arg)`