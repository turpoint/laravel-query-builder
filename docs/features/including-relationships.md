---
title: Including relationships
weight: 3
---

The `include` query parameter will load any Eloquent relation or relation count on the resulting models.
By default, no includes are allowed. All includes must be explicitly specified using `allowedIncludes()`.

For more advanced use cases, custom includes can be used.

```php
// GET /users?include=posts

$users = QueryBuilder::for(User::class)
    ->allowedIncludes(['posts'])
    ->get();

// $users will have all their their `posts()` related models loaded
```

You can load multiple relationship by separating them with a comma:

```php
// GET /users?include=posts,permissions
$users = QueryBuilder::for(User::class)
    ->allowedIncludes(['posts', 'permissions'])
    ->get();

// $users will contain all users with their posts and permissions loaded
```

When trying to include relationships that have not been allowed using `allowedIncludes()` an `InvalidIncludeQuery` exception will be thrown. Its exception message contains the allowed includes for reference.

## Nested relationships

You can load nested relationships using the dot `.` notation:

```php
// GET /users?include=posts.comments,permissions

$users = QueryBuilder::for(User::class)
    ->allowedIncludes(['posts.comments', 'permissions'])
    ->get();

// $users will contain all users with their posts, comments on their posts and permissions loaded
```

## Including related model count

## Custom includes

## Selecting included fields

You can select only some fields to be included using the [`allowedFields` method on the query builder](https://docs.spatie.be/laravel-query-builder/v2/features/selecting-fields/).

⚠️ `allowedFields` must be called before `allowedIncludes`. Otherwise the query builder wont know what fields to include for the requested includes and an exception will be thrown.


## Include casing

Relation/include names will be converted to camelCase when looking for the corresponding relationship on the model. This means `/users?include=blog-posts` and  `/users?include=blogPosts` will both try to load the `blogPosts()` relationship.

## Eloquent API resources

Once the relationships are included, we'd recommend including them in your response by using [Eloquent API resources and conditional relationships](https://laravel.com/docs/5.5/eloquent-resources#conditional-relationships).