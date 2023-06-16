# Eloquent ORM in Laravel

Eloquent is Laravel's object-relational mapping (ORM) system that provides a convenient and expressive way to interact with your database. It allows you to work with database tables using PHP objects and provides methods for querying, inserting, updating, and deleting records. In this README, we will explore the basics of using Eloquent in Laravel.

## Table of Contents
- [Introduction to Eloquent](#introduction-to-eloquent)
- [Defining Eloquent Models](#defining-eloquent-models)
- [Retrieving Records](#retrieving-records)
- [Creating and Updating Records](#creating-and-updating-records)
- [Deleting Records](#deleting-records)
- [Querying Relationships](#querying-relationships)

## Introduction to Eloquent

Eloquent follows the active record pattern, where each database table is represented by a corresponding "model" class. The model class provides a set of methods that allow you to interact with the database table.

Eloquent provides several features, including:

- **Object-Relational Mapping**: Eloquent maps database tables to PHP objects, allowing you to work with data using object-oriented principles.
- **Database Querying**: Eloquent provides a fluent query builder interface that makes it easy to write complex database queries.
- **Relationships**: Eloquent supports defining relationships between models, such as one-to-one, one-to-many, and many-to-many relationships.
- **Mass Assignment Protection**: Eloquent includes a mechanism to protect against mass assignment vulnerabilities.
- **Accessors and Mutators**: Eloquent allows you to define accessors and mutators to modify attribute values when getting or setting them.
- **Timestamps**: Eloquent automatically manages created_at and updated_at timestamps for records.
- **Soft Deleting**: Eloquent supports soft deleting, where records are not permanently deleted but marked as deleted.
- **Query Scopes**: Eloquent allows you to define reusable query scopes to encapsulate common query logic.

## Defining Eloquent Models

To work with a database table using Eloquent, you need to define a model class. Models are typically stored in the `app/Models` directory, but you can place them anywhere you prefer.

To create a new model, you can use the `make:model` Artisan command:

```shell
php artisan make:model User
```

This will generate a new `User` model class in the `app/Models` directory. Open the generated model class and customize it according to your needs.

```php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    protected $table = 'users';
}
```

In the example above, we define the `User` model class that represents the `users` table in the database. By default, Eloquent assumes that the table name is the plural form of the model's name. You can specify a different table name using the `$table` property.

## Retrieving Records

Once you have defined a model, you can use it to retrieve records from the corresponding database table. Eloquent provides various methods for querying records.

### Retrieving All Records

To retrieve all records from a table, you can use the `all` method on the model:

```php
$users = User::all();
```

The `all` method returns a collection of all records in the table.

### Retrieving a Single Record by ID

To retrieve a single record by its primary key (ID), you can use the `find` method:

```php
$user = User::find(1);
```

The `find` method returns a single model instance corresponding to the given ID. If no record is found, `null` is returned

.

### Querying with Conditions

Eloquent provides a fluent query builder interface to specify conditions for querying records. You can chain methods to build complex queries:

```php
$users = User::where('status', 'active')
            ->orderBy('created_at', 'desc')
            ->get();
```

In the example above, we query users with an active status, order the results by the `created_at` column in descending order, and retrieve the records using the `get` method.

### Querying with Relationships

Eloquent allows you to query records based on their relationships. For example, if a `User` model has a `posts` relationship defined, you can retrieve users who have at least one post:

```php
$users = User::has('posts')->get();
```

In this case, the `has` method filters the users based on the existence of related posts.

For more advanced querying options and methods, refer to the [Laravel documentation on Eloquent](https://laravel.com/docs/eloquent#retrieving-models).

## Creating and Updating Records

Eloquent provides methods to create and update records in the database.

### Creating Records

To create a new record, you can create a new instance of the model and set its attributes, then call the `save` method:

```php
$user = new User;
$user->name = 'John Doe';
$user->email = 'john@example.com';
$user->save();
```

The `save` method persists the new record to the database.

Alternatively, you can use the `create` method to create a record in a single line:

```php
$user = User::create([
    'name' => 'John Doe',
    'email' => 'john@example.com',
]);
```

The `create` method mass assigns the given attributes and saves the record.

### Updating Records

To update an existing record, you can retrieve it, modify its attributes, and call the `save` method:

```php
$user = User::find(1);
$user->name = 'Jane Doe';
$user->save();
```

The `save` method updates the record in the database.

Alternatively, you can use the `update` method to update one or more records in a single line:

```php
User::where('status', 'inactive')->update(['status' => 'active']);
```

The `update` method applies the changes to the specified records.

## Deleting Records

Eloquent provides methods to delete records from the database.

### Deleting a Single Record

To delete a single record, you can retrieve it and call the `delete` method:

```php
$user = User::find(1);
$user->delete();
```

The `delete` method removes the record from the database.

### Deleting Multiple Records

To delete multiple records, you can use the `destroy` method:

```php
User::destroy([1, 2, 3]);
```

The `destroy` method accepts an array of primary key values and deletes the corresponding records.

### Soft Deleting

Eloquent supports soft deleting, where records are not permanently deleted but marked as deleted. To use soft deleting, add the `Illuminate\Database\Eloquent\SoftDeletes` trait to your model:

```php
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\SoftDeletes;

class User extends Model
{
    use SoftDeletes;

    // ...
}
```

Once the trait is added, you can use the `delete` method as usual, and the record will be marked as deleted. To retrieve records including soft deleted ones, you can use the `withTrashed` method:

```php
$users = User::withTrashed()->get();
```

For more information on soft deleting and working with deleted records, refer

 to the [Laravel documentation on soft deletes](https://laravel.com/docs/eloquent#soft-deleting).

## Querying Relationships

One of the powerful features of Eloquent is its ability to query relationships between models.

### Retrieving Related Models

To retrieve related models, you can access the relationship methods defined in your model. For example, if a `User` model has a `posts` relationship, you can retrieve all posts for a user:

```php
$user = User::find(1);
$posts = $user->posts;
```

In this example, the `posts` relationship returns a collection of related `Post` models.

### Eager Loading Relationships

By default, when you retrieve a model with its relationships, Eloquent performs separate queries for each relationship. This can result in the "N+1 problem," where the number of queries grows linearly with the number of records.

To avoid this issue, you can use eager loading to load relationships with fewer queries. You can specify which relationships to eager load using the `with` method:

```php
$users = User::with('posts')->get();
```

In this example, Eloquent loads the users and their related posts in a single query.
