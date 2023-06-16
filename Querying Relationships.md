# Querying Relationships
When working with Eloquent relationships in Laravel, you can query related models using various methods and techniques. In this section, we will explore different ways to query relations.

## Eager Loading

Eager loading allows you to retrieve related models in a more efficient way, reducing the number of database queries. Instead of retrieving related models one by one, you can eager load them using the `with` method.

```php
$users = User::with('posts')->get();
```

In this example, the `with('posts')` method eager loads the `posts` relationship for all the retrieved `User` models. This helps to avoid the N+1 problem, where additional queries would be executed for each user to fetch their posts.

## Querying the Relationship

You can also query the relationship itself to retrieve specific related models based on certain conditions. For example, to retrieve users who have at least one published post:

```php
$users = User::has('posts', '>', 0)->get();
```

In this case, the `has('posts', '>', 0)` method filters the users based on the existence of at least one post.

## Querying Through Relationships

You can also query through relationships to retrieve related models based on conditions of the related model. For example, to retrieve users who have at least one published post in the last week:

```php
$users = User::whereHas('posts', function ($query) {
    $query->where('status', 'published')
          ->whereBetween('created_at', [now()->subWeek(), now()]);
})->get();
```

In this example, the `whereHas` method filters the users based on the existence of posts that satisfy the specified conditions.

## Querying Polymorphic Relationships

When working with polymorphic relationships, you can query related models using the `morphTo` and `morphMany` methods. For example, to retrieve all comments associated with a specific post:

```php
$post = Post::find(1);
$comments = $post->comments;
```

In this case, the `$post->comments` statement retrieves all the comments associated with the post.
