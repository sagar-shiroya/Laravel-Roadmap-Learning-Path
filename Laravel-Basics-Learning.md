# Routing and Controllers: Basics

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
