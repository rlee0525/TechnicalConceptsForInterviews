# JavaScript: Basics and Tricky Questions

> Contributor: Torah Oglander

### What are the differences between null and undefined?
- `null`: No value. Null can be assigned to any variable but is not an object. typeof null returns `object`
- `undefined`: Undefined means the variable has not been declared yet, or the property doesn't exist on
the object

### What is a closure?
- All JS functions have pointers to the variables that are in scope at the time of creation
- JS functions maintain pointers to variables that they use
- By returning a function from within a function, variables scoped to the outer function
can be maintained as 'enclosed' (private) variables in the returned function
- This technique is often used to create private variables that persist for the duration
of a program's execution. For example, this is an effective way to do achieve memoization
in JavaScript

Node is just JavaScript outside of the browser.

In the browser, Window is the global object.

In node, 'Global' is the global object.

However, you can not add variables to global implicitly in node the way all variables
get added to Window in the browser. Instead local variables are namespaced to the file
in which they are declared.

### What is Node.js ?

- Node is actually any JavaScript run outside of the browser, but commonly refers
to using JavaScript as a backend framework. The followng notes outline some basic
concepts for developing backends with Node.js

\_\_dirname is a special variable that gets the path to current directory \n
\_\_filename is a special variable that gets the path to the current file

#### -PROCESS-

The 'process object' is a useful tool in node.
process.argv is always available, and contains an array that lists the paths to the current
file and directory, and any command line arguments that were passed in at the time of calling.
With process.argv, you can customize your function's behavior based on command line flags.

process also contains methods for handling stin and stio, so you can do things like process.stin
or process.stout.write("hello wold")

the 'require' function is built in to node, and can be used to import code into a project.

the 'path module' needs to be required in and usually gets required in to a local variable creatively names
'path'.

'path', 'process' and 'util' are commonly used node modules

The v8 module give you access to memory use. you can call v8.getHeapStatistics() to get some information
about the memory in use in your computer.

#### -Readline-

The readlines module will allow you to get access to inputs from the user.

readline.createInterfact(process.stdin, process.stout) will return a readline instance.

You can then call .question() on that instance, which takes two arguments. First, a string to display
in the terminal, and second a callback that handles the response.

You can update the prompt string by calling setPrompt on that readline instance. Then call .prompt() to
prompt again.

You can also listen for the 'line' event on your readline object, which will be fired when the user hits
return.

You can also listen for the 'close' event, which allows you to handle closing the readline with process.exit().

Call readline.close() to end the listening, and process.exit() to kill it off.

#### -EVENT EMITTER-

The eventEmitter module implements the publish-subscribe ('pub-sub') design pattern in JavaScript.

create an eventEmitter instance by requiring in the 'events' module, and then calling events.EventEmitter()

You can then add event handling by calling .on on the event emitter instance, passing a string for the event's
name, and a callback telling the emitter how to handle that event.

You can then emit the event by calling .emit() on the emitter with the event type as a string as the first
argument. Subsequent arguments will be passed along to the handler callback.

Any objects that you create that inherit from EventEmitter can similarly be set to listen for specific events by
calling .on() on the object. .on() takes two arguments, the event name (String) and a callback.

#### -COMMON JS-

In ES5, you can require in built-in modules by adding:
var someVariable = require('moduleName');

You can also require your own locally defined modules with:
var myVar = require('./PATH_TO_SOURCE');

The result of this type of require statement depends completely on what is exported from
the source file. You control what gets exported with module.exports. Whatever you set
module.exports equal to will be returned when require is called externally.

#### -CHILD_PROCESS-

.exec

The 'child_process' module allows you to execute file system commands (terminal commands that would
normally be typed into the terminal prompt) from within your JS code.

To use this module, create a local instance of child_process by setting a require('child_process')
call to a variable. Then that variable's .exec() function will execute whatever you pass in as a string
as if it had been typed in the command line. You can also handle the response that comes from the prompt
with a callback that is passed in as a second argument.

.spawn

.spawn is another child_process function that allows you to run additional processes from within
a node.js script. The call to .spawn takes a string command as an argument, with optional arguments
following the command string. The command passed in will be executed asynchronously.

#### -FS-

The fs module is used to access the file directory. All of the commands defined in the fs
module have both synchronous and asynchronous forms. There are many commands, which can be found
online.

fs.readfileSync and fs.readFile (async) both take a string path to a file as their first argument,
and an encoding (also a string) that is required to view anything other than a binary chunk. The async
versions of these functions take callback as the third argument. However, these functions will read the
entire file into memory at once, so they will execute slowly, and may cause crashes with very large files.

fs.stats() and fs.statSync() take a path as their first arguments, and get statistics about the file.

The fs module can also be used to create new files. fs.writeFile() and fs.writeFileSync() take a
string for the file name as the first argument, and the contents to put in the file (also a string) as
a second argument. The async version also takes a callback.

fs.appendFile() is similar to writeFile, but it adds to already existing files instead of creating a new one.

fs.mkdir() will create a directory. It takes a string as its first argument, that will become the name of the
new directory.

fs.existsSync() will return a boolean value and tell you if the passed in string refers to a file or directory
in the current working directory.

fs.renameSync() and fs.rename() will take two arguments, the old name (relative path) is the first argument,
and the new name (relative path) is the second argument. The file will be renamed, and potentially moved.
The async version takes a callback as the third argument.

fs.unlink() and .unlinkSync() take a file name (path) as their first argument, and will delete that file.

Errors with fs can be handled in callbacks, or caught with try / catch blocks.

fs.rename() and fs.renameSync() can move and rename directories in just the same way as files.

fs.rmdir() and fs.rmdirSync() will remove a directory, but only if it's empty. If the directory contains
files, they must all be removed first, before rmdir will work. Use fs.unlink() to remove files from
the directory.

#### -STREAM_INTERFACE-

The stream interface breaks data into smaller chunks for more efficient processing. The process.stin and
process.stout functions both implement this interface.

fs.createReadStream() can help you read from a large file without buffering huge amounts of data all at
once. The first argument it takes is a file path and the second is an encoding such as 'UTF-8'.
fs.createReadStream() returns a stream object, which can be set to listen for events with .on(). You can
add event listeners like so:

```javascript
const stream = fs.createReadStream('./path_to_file', 'UTF-8');

stream.once('data', function() { handle first data chunk })
stream.on('data', function(dataChunk) { ...do something with data chunk... });
stream.on('end', function() { ...handle end of file reading...})
```

fs.createWriteStream('New_File_Name') returns a stream writer, which has a write() method that will add text
to a file. Then, call .close() on the stream to tie it off.

#### -HTTP- and -HTTPS-

http and https modules can handle requests and be used to create servers. The https module requires
a security certificate as an argument.

both the http and https modules require an options hash. The hash is a pojo with at least a hostname:
and port: key. The port 80 is common for http and 443 is common for https. Other keys include method: and
path: . The string set for path will be appended to the end of the hostname:

To make an http (or https) request, call request() on the http(s) module, and pass in your options hash
as the first argument and a callback as the second. The callback will receive a response, which will be
the response to the request.

The request method implements a stream, so the response may come back as a series of data chunks if it
is large.

```javascript
// Example (ES5) - THIS DIDN'T WORK FOR ME!!!
var https = require('https');
var fs = require('fs');

var options = {
  hostname: "en.wikipedia.org",
  port: 443,
  path: '/wiki/George_Washington',
  method: "GET",
};

var req = https.request(options, function(res) {
  var responseBody = ""; // start with an empty string and build on this packet by packet

  console.log("Response started...");
  console.log(`Server status: ${res.statusCode}`); //mostly es5
  console.log("Response headers: %j", res.headers);

  res.setEncoding('UTF-8'); // You MUST set encoding or it will be binary.

  res.once('data', function(chunk) { // give one sample chunk to see
    console.log(chunk);
  });

  res.on('data', function(chunk) { // add all packets to response body
    responseBody += chunk;
    console.log("chunk, length: ", chunk.length);
  });

  res.on('end', function() {
    fs.writeFile('George_Washington.html', responseBody, function(err) {
      if (err) {
        throw err; // throw err to alert user if problem occurs. DO NOT write file.
      }
      console.log("file downloaded");
    })
  });
};
```

#### BUILDING SERVERS with HTTP and HTTPS

both http module and https can respond to requests. HTTPS will require a security
certificate.

These modules (http and https) both have .createServer() functions that return
a server instance. This function takes a callback, which will be invoked
every time a request comes in. This callback requires two arguments,
request and response. Request just stands in for whatever request will eventually
come in. Response will be sent back when a request is received. Within the body
of this callback you can set headers with response.writeHead(status_code, { headed })

You can pass in the response as a string literal after writing headers.
Pass the response string to a call to .end() attached to the response variable
inside the body of the callback. This tells the server how to finish handling a request.

Call .listen(port_number) on the server instance to get ready to handle requests.

In Node.js it is the developer's responsibility to handle all anticipated
request types, and return the appropriate response. This often requires combining
the fs module and the http or https modules.

Within the callback passed to createServer, you can access the requested url string
request.url. Use this to determine how you want to process the request.

Node allows you to build your own server, which is more work, but allows for great
flexibility.

You can also build a JSON API with the http or https modules by adding a call to
JSON.stringify of the data you want to send back to the .end() call.

In much the same way that you can handle different routes by inspecting the
request.url to determine what to send in your response, you can also handle different
request types by inspecting the request.method. If it is 'GET' (req.method === "GET"),
you can do one thing, but if it is 'POST' (req.method === "POST") do whatever
else you need to.

#### -EXPRESS-

Express is the most popular framework for building websites with Node. Although you can
roll your own using the fs and http or https modules, using a framework like Express
make the development process faster and smoother.

The basics of express start with installing express as a dependency via NPM (or Yarn).
Then, in your JavaScript, you will need to require or import Express. You can then
save your express server into a variable by calling express() without passing any
arguments.

You can add middleware to your express server by calling .use(callback) on the variable
that holds your server. The callback takes three arguments: request, response, and next.
The request and response arguments are just like the ones in the http and https modules.
The next argument is a function that must be invoked to move on to the next piece of
middleware (or move on from middleware altogether). Express also comes with some pre-fabricated
middleware that you may want to use, such as express.static('path') which is a static
file server that will serve up standard files such as .css an .png type files.

Here is an example of a simple express server:

```javascript
var express = require('express');

var app = express();

// this is custom middleware that logs the request method and url
app.use(function(req, res, next) {
  console.log(`${req.method} request for ${req.url}`);
  next();
})

app.use(express.static('./public')); // this is built-in static file server middleware

app.listen(3000);

console.log('express server running on port 3000');

module.exports(app); // it is a good idea to export your server so it can be accessed from other files.
```

With the CORS module, you can add cross-origin resource sharing to your app.
Simply install and then require the module, and then call cors() as a middleware
after setting up your request handlers in your code.

#### -Web Sockets-

The ws module can be used to implement WebSockets with Node.js. Require it
and instantiate a WebSocketServer by calling ```new ws.Server()```

It is also possible to use the socket.io module with express to create a
more robust websocket server with polyfill support.

#### -MOCHA-

Mocha and Chai are standard test libraries that work well with Node.js.
The standard setup requires a 'test' directory in you root project folder.
 will need to install Mocha globally in your system, and install Chai as
 a dev dependency in your project. Mocha is a testing sweet and Chai provides
 the matching functions that are required to evaluate the test results.
 You can require them into your test file as Chai.expect, Chai.should,
 and Chai.assert. See the official documentation for more information:
 [Mocha](https://mochajs.org/), [Chai](http://chaijs.com/)

When writing unit tests for API calls you can use nock.js to create mock-server
endpoints. This will speed up the process of running the unit tests, and
ensure adequate modularity for more precise testing. Learn more from their
github page: [nock.js](https://github.com/node-nock/nock)

You can use [Istanbul](https://istanbul.js.org/) for checking code coverage.
To use it install Istanbul globally, and then call Instanbul _mocha.

[Supertest](https://github.com/visionmedia/supertest) is a helpful library
for testing api endpoints.

[rewire](https://github.com/jhnns/rewire) is a library that lets you inject
mocks into your tests so that you can focus on the SUT (system under test)
without depending on external data, and also avoid mutating external state
when testing.

[Sinon](http://sinonjs.org/) is a tool for building mocks and checking
on their state which is very helpful while testing. The Sinon library has
a concept called a Sinon spy which allows you explore attributes of the mocks
under test. Stubs are another useful feature in this library.

#### -Grunt-

[Grunt](https://gruntjs.com/) is a task runner that allows you to define tasks, and then manage them with functionality somewhat similar to webpack.

In order to use grunt, you need to create a file called 'Gruntfile.js'. The Gruntfile must export a function that takes a 'grunt' object as its first argument. Within that function you will want to first attach a .initConfig property
which is POJO with tasks as keys and the configuration details for that task as the values for the keys.

There are many tasks available, and you will need to learn how to use and configure each one since they are all structured differently. For example css preprocessors are available, jshint is available and transpiling is also available.

After the .initConfig, you also want to attach .loadNPMTask calls and .registerTask calls to the grunt object within this
exported function. Loading makes the tasks available to Grunt, and then registering will specify how you can run the tasks from the command line.

```javascript
grunt.loadNPMTask('grunt-contrib-taskName');

grunt.registerTask('default', ['taskName']);
```

will load the task into grunt, and then call it when you type grunt at the command line.

There are many things tasks that can be handled with Grunt, so read the docs and google for details.

#### -NPM Scripts-

Inside package.json, you can define scripts that can speed up your workflow. Some script names are predefined, such as "start", you can specify what will be done when someone types npm start at the command line by adding a quoted string as the value to the key "start" inside the key "scripts" object in package.json. The string describing the script can include anything that you might type in at the command line. For example "node server.js" would run the code in the file server.js with node. You can also define a prestart key, and the string specified as its value will be executed immediately before the 'start' script.

For predefined keys like start, you can run them with npm key. However, if you want to generate your own script names that are not in the set of predefined options, you can. WHen you want to run these, you will need to type 'npm run MY_SCRIPT' instead of just 'npm script'.

The string that defines the script may have multiple calls concatenated with &.

So, for example:

```javascript
...inside package.json
"scripts": {
  "predev": "grunt", // runs grunt before 'dev'
  "dev": "open http://localhost:3000 & node-dev app & grunt watch"
}
```

will do the following things after you type 'npm run dev' at the command line:
1. run grunt once
2. open a browser window to http://localhost:3000
3. start, serve and watch the contents of app (a node server...)
4. run grunt watch to update your bundled code as you go

You can add postscript commands just like the prescript ("predev") command shown above. As you should expect, these will execute immediately after the script.
