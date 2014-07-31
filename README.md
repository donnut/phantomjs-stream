# phantomjs-stream

Node streams and phantomjs live together very nicely. The only requirement is a wrapper that handles the io of the external phantomjs process, something like [event-stream](https://github.com/dominictarr/event-stream).

## Hello world!
The node script takes stdin, pipes it through phantomjs and pipes the result to stdout.

```javascript
// myStream.js
var es = require('event-stream');
var cp = require('child_process');

process.stdin
.pipe(es.child(cp.exec('/usr/bin/phantomjs echo.js')))
.pipe(process.stdout);
```
It is assumed that phantomjs lives in `/usr/bin/`.

```javascript
// echo.js
var system = require('system');

var line = system.stdin.readLine();
system.stdout.writeLine(JSON.stringify(line));

phantom.exit(0);
```
Call the script like:
```
echo Hello World | node echo.js
```
and you should get:
```
"Hello World!\n"
```
