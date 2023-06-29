# Simple HTTP router

The mentioned package provides a functionality to effectively handle HTTP requests by routing them to specific callback functions. This means that instead of directly accessing and executing a particular URL, the package acts as a mediator, directing the requests to their designated callback functions.

To enable this routing mechanism, the package allows users to register URL paths, HTTP methods, and their corresponding callback functions. By specifying the URL paths and the associated HTTP methods, developers can define the specific routes that should trigger certain actions or logic.

When an HTTP request is made, the package comes into action by processing the incoming request. It compares the requested URL path and HTTP method with the registered routes. If a match is found, the corresponding callback function is invoked, allowing the desired logic or functionality to be executed.

In summary, this package provides a comprehensive solution for handling HTTP requests in an organized and controlled manner. It allows developers to register routes, associate them with callback functions, and efficiently process incoming requests, ensuring the appropriate callback function is invoked based on the matching URL paths and HTTP methods.

## Usage

```php
use Mateodioev\HttpRouter\exceptions\{HttpNotFoundException, RequestException};
use Mateodioev\HttpRouter\{Request, Response, Router};

$router = new Router();

// Register your endpoints here
$router->get('/', function (Request $r) {
    return Response::text('Hello world!');
});

$router->run();
```

### Methods
You can use the `Router::get`, `Router::post`, `Router::put`, `Router::patch` and `Router::delete` methods to register your endpoints.

```php
$router->myHttpMethod($uri, $callback);
```
- `$uri` is the path of the endpoint.
- `$callback` is the function that will be executed when the endpoint is requested. Each callback (action) must return an instance of the Mateodioev\HttpRouter\Response class or an InvalidReturnException will be thrown.

#### Static files
You can map all static files in a directory with the `static` method.

```php
$router->static($baseUri, $path, $methods);
// Default methods are GET
```

Example:

```bash
tree
```
```text
.
├── index.php
└── styles.css

1 directory, 2 files
```

```php
$router->static('/docs', 'public/');
```
Now you can reach this uris

- /docs/index.php
- /docs/styles.css


#### Handling all HTTP methods
You can use the `Router::all` method to handle all HTTP methods with one callback.

```php
$router->all($uri, $callback);
```

### Path parameters

You can use path parameters in your endpoints. Path parameters are defined by a bracket followed by the name of the parameter.
> `/users/{id}`

```php
$router->get('/page/{name}', function (Request $r) {
    return Response::text('Welcome to ' . $r->param('name'));
});
```

**Note:** You can make a parameter optional by adding a question mark after the name of the parameter.
> `/users/{id}?`

```php
$router->get('/page/{name}?', function (Request $r) {
    $pageName = $r->param('name') ?? 'home'; // If the parameter is not present, the method return null
    return Response::text('Welcome to ' . $pageName);
});
```

### Request data

You can get all data from the request with the following methods:

- `Request::method()` returns the HTTP method of the request.
- `Request::uri()` returns the URI of the request.
- `Request::uri()` returns the URL of the request.
- `Request::param($name, $default = null)` returns the value of the parameter with the name `$name` or null if the parameter is not present.
- `Request::params()` return al the request URI parameters.
- `Request::headers()` returns an array with all the headers of the request.
- `Request::body()` returns the body of the request (from [php://input](https://www.php.net/manual/en/wrappers.php.php)).
- `Request::data()` returns an array with all the data of the request. Use this when Content-Type is _application/x-www-form-urlencoded_ or _multipart/form-data_.
- `Request::files()` returns an array with all the files of the request.
- `Request::query()` returns an array with all the query parameters from the uri.


*TODO list:*
- [ ] Add support for middlewares.
- [ ] Add support for custom error handlers.
