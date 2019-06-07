# ExpressJS-Notes

___

## Routing

`Routing` refers to determining how an application responds to a client request to a particular endpoint, which is a URI (or path) and a specific HTTP request method (GET, POST, and so on).

You define routing using methods of the Express app object that correspond to HTTP methods.

These routing methods specify a callback function (sometimes called “handler functions”) called when the application receives a request to the specified route (endpoint) _and_ HTTP method. 

In other words, the application “listens” for requests that match the specified route(s) **and** method(s), and when it detects a match, it calls the specified callback function.

Each route can have one or more handler functions, which are executed when the route is matched. (Must provide the `next` argument and call the `next()` function.)


**Route Definition:**  `app.METHOD(PATH, HANDLER)`

Where:

  `app` is an instance of express.
  
  `METHOD` is an HTTP request method, in lowercase.
  
  `PATH` is a path on the server.
  
  `HANDLER` is the function executed when the route is matched.

```js
// Basic route example
var express = require('express');
var app = express();

// respond with "hello world" when a GET request is made to the homepage
app.get('/', function (req, res) {
  res.send('hello world')
});
```

___

### Route Methods

A route method is derived from one of the HTTP methods, and is attached to an instance of the `express` class.

The following example shows routes that are defined for the `GET` and the `POST` methods to the root of the app.

```js
var express = require('express');
var app = express();

// GET method route
app.get('/', function (req, res) {
  res.send('GET request to the homepage')
});

// POST method route
app.post('/', function (req, res) {
  res.send('POST request to the homepage')
});
```

___

### Route Paths

Route paths, in combination with a request method, define the endpoints at which requests can be made. 

Route paths can be strings, string patterns, or regular expressions.

Query strings are not part of the route path.

Examples:
```js
// String based route path
app.get('/about', function (req, res) {
  res.send('about');
});

//  This route path will match requests to /about.
```

```js
// String pattern route path
app.get('/ab+cd', function (req, res) {
  res.send('ab+cd');
});

//  This route path will match abcd, abbcd, abbbcd, and so on.
```

```js
// Regular expression route path
app.get(/.*fly$/, function (req, res) {
  res.send('/.*fly$/');
});

//  This route path will match butterfly and dragonfly, but not butterflyman, dragonflyman, and so on.
```

___

### Route Parameters

Route parameters are named URL segments that are used to capture the values specified at their position in the URL. 

The captured values are populated in the `req.params` object, with the name of the route parameter specified in the path as their respective keys.

```
Route path: /users/:userId/books/:bookId
Request URL: http://localhost:3000/users/34/books/8989
req.params: { "userId": "34", "bookId": "8989" }
```

To define routes with route parameters, simply specify the route parameters in the path of the route as shown below.

```js
app.get('/users/:userId/books/:bookId', function (req, res) {
  res.send(req.params)
});
```

___

### Route Handlers

You can provide multiple callback functions that behave like middleware to handle a request.

Route handlers can be in the form of a function, an array of functions, or combinations of both.

```js
// An array of callback functions to handle the route '/example/c'.
var funcA = function (req, res, next) {
  console.log('funcA');
  next();
};

var funcB = function (req, res, next) {
  console.log('funcB');
  next();
};

var funcC = function (req, res) {
  res.send('Hello from funcC!');
}

app.get('/example/c', [funcA, funcB, funcC]);
```

___

### Response Methods

The following methods on the response object, (`res`), can send a response to the client, and terminate the request-response cycle. 

If none of these methods are called from a route handler, the client request will be left hanging.

`res.end()` - End the response process.

`res.json()` - Send a JSON response.

`res.redirect()` - Redirect a request.

`res.render()` - Render a view template.

`res.send()` - Send a response of various types.

`res.sendFile()` - Send a file as an octect stream

`res.sendStatus(`) - Set the response status code and send it as a string in the response body.

___

### Simple ExpressJS Server

```js
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => res.send('Hello World!'));

app.listen(port, () => console.log(`Example app listening on port ${port}!`));
```

___

### Serving Static Files In ExpressJS

To serve static files such as images, CSS files, and JavaScript files, use the `express.static` built-in middleware function in Express.

The function signature is:

```js
express.static(root, [options]);
```

The `root` argument specifies the root directory from which to serve static assets.

For example, use the following code to serve images, CSS files, and JavaScript files in a directory named `public`:

```js
app.use(express.static('public'));
```

Now, you can load the files that are in the public directory:

```
http://localhost:3000/images/kitten.jpg
http://localhost:3000/css/style.css
http://localhost:3000/js/app.js
http://localhost:3000/images/bg.png
http://localhost:3000/hello.html
```

**Note:** Express looks up the files relative to the static directory, so the name of the static directory is not part of the URL.

To create a virtual path prefix (where the path does not actually exist in the file system) for files that are served by the express.static function, specify a mount path for the static directory, as shown below:

```js
app.use('/static', express.static('public'));
```

Now, you can load the files that are in the `public` directory from the `/static` path prefix.

```
http://localhost:3000/static/images/kitten.jpg
http://localhost:3000/static/css/style.css
http://localhost:3000/static/js/app.js
http://localhost:3000/static/images/bg.png
http://localhost:3000/static/hello.html
```

However, the path that you provide to the `express.static` function is relative to the directory from where you launch your `node` process. 

If you run the express app from another directory, it’s safer to use the absolute path of the directory that you want to serve:

```js
app.use('/static', express.static(path.join(__dirname, 'public')))
```



