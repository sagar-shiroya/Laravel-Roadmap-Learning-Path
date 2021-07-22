# **Routing and Controllers: Basics**

## **Basic Routing**

This is how most basic Laravel Routes looks like. It simply accepts URI & Closure.

```php
Route::get($uri, $callback);
```

Below is the example of how it looks:

```php
use Illuminate\Support\Facades\Route;

Route::get('/greeting', function(){
    return 'Hello World';
});
```

There are **four** different type of routes inside the route directory:

- web.php (Route for web interface)
- api.php
- channels.php
- console.php

 **Different methods for routes:**

```php
Route::get($uri, $callback);
Route::post($uri, $callback);
Route::put($uri, $callback);
Route::patch($uri, $callback);
Route::delete($uri, $callback);
Route::options($uri, $callback);
```

### Dependency Injection

If any dependencies are required in the route callback then it can be resolved by injecting it to the callback function.

```php
use Illuminate\Http\Request;

Route::get('/users', function (Request $request) {
    // ...
});
```

### **CSRF Protection**

Any HTML forms having POST, PUT, PATCH, DELETE web routes then form should include CSRF token field. Otherwise the request will be rejected with Error 503.

```HTML
<form method="POST" action="/profile">
    @csrf
    ...
</form>
```

### **Redirect Routes**

If you are defining routes that redirects to another URI then use the `Route::redirect` method.

```php
Route::redirect('/here', '/there'); //e.g. user will enter http://localhost/here and it will redirect them to http://localhost/there 
```

By default, above method returns `302` status code. But if you want to customize the status code then pass it as third optional parameter:

```php
Route::redirect('/here', '/there', 301);
```

`Route::permanentRedirect` method return a 301 status code by default.

 ```php
Route::permanentRedirect('/here', '/there');
```

## **View Routes**

If your route needs to return only view then you may use the `Route::view` method like redirect method. This method only accepts a URI as first argument and view name as second argument. As third option, you can also pass the array of data to view.

```php
//View route sytax
Route::view($uri, $viewname);

//Simple view route example
Route::view('/welcome', 'welcome');

//View route with optional data array as third parameter
Route::view('/welcome', 'welcome', ['name' => 'Taylor']);
```

## **Basic controllers with Routes**

Controller extends the base controller class included in Laravel.

```php
namespace App\Http\Controllers;

use App\Http\Controllers\Controller;

class ExampleController extends Controller
{
    public function action($params)
    {
        // ...
    }
}
```

Route for above controller will look like below:

```php
use App\Http\Controllers\ExampleController;

Route::get('/example/{params}', [ExampleController::class, 'action']);
```

When an incoming request matches the specified route URI, the `action` method of the  `App\Http\Controllers\ExampleController` class will be invoked.

## **Route Parameters**

Sometimes you need to capture some details from the URI into your route. E.g. you need to capture ID from the route. Here's how you can do it:

```php
Route::get('/user/{id}', function ($id) {
    return 'User' . $id;
});
```

### **Multiple Route Parameters**

Multiple route parameters can also be defined. There's no restriction on it.

```php
Route::get('/posts/{post}/comments/{comment}', function ($postId, $commentId) {
    // ...
});
```

This parameters always:

- Enclosed within { }
- Consists of alpahbetic characters and underscores(optional)
- Injected into route callbacks/controllers based on their order & name of the route callback/controller argument doesn't matter

### **Route parameters with dependencies**

If you have dependencies to inject along with route parameters then list your route parameters after injection of dependencies:

```php
use Illuminate\Http\Request;

Route::get('/user/{id}', function (Request $request, $id) {
    return 'User '.$id;
});
```

### **Optional Route Parameters**

If you need to specify optional route parameters then you can do by placing a `?` after the parameter name. Make sure to provide a default value for corresponding variable.

```php
Route::get('/user/{name?}', function ($name = null) {
    return $name;
});

Route::get('/user/{name?}', function ($name = 'John') {
    return $name;
});
```

### **Validate Route Parameters(with regex helper)**

`where` method can be used to constraint the route parameter on a route instance. `where` method accepts the name and regular expression as parameter:

```php
Route::get('/user/{id}', function($id){
    //...
})->where('id','[0-9]+');

Route::get('/user/{name}', function($name){
    // ...
})->where('name','[A-Za-z]+');

Route::get('/user/{id}/{name}',function($id, $name){
    //...
})->where(['id'=>'[0-9]+', 'name'=>'[A-Za-z]+']);
```

For convenience, some of the regular expressions have helper methods which can be directly used.

- whereNumber( ) (pattern: [0-9]+)
- whereAlpha( ) (pattern: [A-Za-z]+)
- whereAlphaNumric( ) (pattern: [A-Za-z0-9]+)

----------

## **Blade Basics**

If you want to display data passed through view routes in blade template then you can use that variable by wrapping it with curly braces.

```php
//Route File
Route::get('/', function () {
    return view('welcome', ['name' => 'Samantha']);
});

//Blade Template
Hello, {{name}}
```

*Note: Blade's {{ }} statement automatically passed through PHP's `htmlspecialchars` function to prevent XSS attacks.*

You can also put any PHP function or PHP code wish to print inside blade echo statement:

```PHP
The current UNIX timestamp is {{ time() }}.
```

### **Rendering JSON**

Sometimes if you want to pass variable to view in intention of rendering it as JSON in oreder to initialize as JS variable.

```PHP
<script>
    var app = <?php echo json_encode($array); ?>;
</script>
```

Instead of calling `json_encode()`, we can use the JSON Blade directive `@json`. It accepts the same arguments as PHP's json_encode()  function.

```PHP
<script>
    var app = @json($array);
    // OR
    var app = @json($array, JSON_PRETTY_PRINT);
</script>
```

Note: Double curly braces automatically passes the data thorugh htmlspecialchars function to prevent XSS attacks. If you don't want to escape it, you may use the below syntax:

```PHP
Hello, {!! $name !!}.
```

@ symbol used to escape the Blade directives.

```PHP
{{-- Blade template --}}
@@json()

<!-- HTML output -->
@json()
```
