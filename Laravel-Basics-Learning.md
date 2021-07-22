# Routing and Controllers: Basics

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

## Dependency Injection

If any dependencies are required in the route callback then it can be resolved by injecting it to the callback function.

```php
use Illuminate\Http\Request;

Route::get('/users', function (Request $request) {
    // ...
});
```
