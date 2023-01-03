# Laravel : Descriptive API methods
[![Latest Stable Version](https://poser.pugx.org/sarav/descriptive-api/v/stable)](https://packagist.org/packages/sarav/descriptive-api) [![Total Downloads](https://poser.pugx.org/sarav/descriptive-api/downloads)](https://packagist.org/packages/sarav/descriptive-api) [![Latest Unstable Version](https://poser.pugx.org/sarav/descriptive-api/v/unstable)](https://packagist.org/packages/sarav/descriptive-api) [![License](https://poser.pugx.org/sarav/descriptive-api/license)](https://packagist.org/packages/sarav/descriptive-api)	




A Simple Laravel Package providing descriptive API methods

- **Laravel**: 9.0.*
- **Author**: Sarav
- **Author Homepage**: http://sarav.co


## Install Composer package ##

Open your terminal and navigate to your laravel folder. Now run the following command

	composer require sarav/descriptive-api

That's all is required for the package setup.

## Usage

Following are the list of available methods

| Methods                | Status Code   |  Default Message                         |
|------------------------|---------------|------------------------------------------|
| `ok()`                 | 200           |                                          |
| `created()`            | 201           |                                          |
| `noContent()`          | 204           |                                          |
| `badRequest()`         | 400           | Invalid Request                          |
| `unauthorized()`       | 401           | User unauthorized                        |
| `forbidden()`          | 403           | Access denied                            |
| `notFound()`           | 404           | Resource not found                       |
| `validationFailure()`  | 422           | Validation failure                       |
| `internalServerError()`| 500           | Internal Server Error                    |


Now you can start using the methods as follows

### `ok()`

```php
public function index()
{
    $articles = Article::paginate();
    return response()->ok($articles);
}
```

### `created`

```php
public function store(StoreArticleRequest $request)
{
    $article = Article::create($request->validated());
    return response()->created($article);
}
```

### `noContent`
```php
public function destroy(Article $article)
{
    $article->delete();
    return response()->noContent();
}
```

### `badRequest`
```php
public function store(Request $request)
{
    if ($request->age < 0) {
      return response()->badRequest();
    }
}
```

### `unauthorized`
```php
public function store(Request $request)
{
    if (!auth()->check()) {
      return response()->unauthorized();
    }
}
```

### `forbidden`
```php
public function store(Request $request)
{
    $user = $request->user();
    if (!$user->isAdmin()) {
        return response()->forbidden();
    }
}
```

### `notFound`
```php
public function update(Request $request, $id)
{
    $article = Article::find($id);
    if (!$article) {
        return response()->notFound();
    }
}
```

### `validationFailure()`
```php
public function update(Request $request, $id)
{
    $validator = Validator::make([
      'comment' => 'required'
    ], $request->all());
    
    if ($validator->fails()) {
        return response()->validationFailure('Validation failure', $validator->errors());
    }
}
```

### `internalServerError()`

```php
public function update(Request $request, $id)
{
    try {
        // Your Business Logic
    } catch (Exception $e) {
        return response()->internalServerError();
    }
}
```

## Overriding Default Messages

If you wish to override the default message, then simply provide your default message as param. That's it.

For example,


```php
public function store(Request $request)
{
    // Overriding badRequest()
    if ($request->age < 0) {
      return response()->badRequest('Invalid age provided');
    }
    
    // Overriding unauthorized()
    if (!auth()->check()) {
      return response()->unauthorized('User not authorized');
    }
  
    // Overriding forbidden()
    $user = $request->user();
    if (!$user->isAdmin()) {
        return response()->forbidden('Access denied. Please contact administrator');
    }
    
    // Overriding notFound()
    $article = Article::find($id);
    if (!$article) {
        return response()->notFound('Article not found');
    }
    
    // Overriding validationFailure()
    $validator = Validator::make([
      'comment' => 'required'
    ], $request->all());
    
    if ($validator->fails()) {
        return response()->validationFailure('Please check the errors', $validator->errors());
    }
}
```

For more information <a href="https://sarav.co/descriptive-api-response-methods-in-laravel" target="_blank">check out this article</a>.


