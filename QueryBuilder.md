# Query Builder
The Query Builder in Laravel provides a fluent interface for building and executing database queries. It allows you to construct complex SQL queries in a programmatic and database-agnostic way. Here's an overview of how to use the Query Builder in Laravel.

## Basic Query Building

To start building a query using the Query Builder, you can use the `DB` facade to access the Query Builder instance. Here's an example of selecting all records from a table:

```php
$users = DB::table('users')->get();
```

This query retrieves all records from the "users" table and returns them as a collection of stdClass objects.

You can chain methods to build more complex queries. For example, to select specific columns, apply conditions, and order the results:

```php
$users = DB::table('users')
            ->select('id', 'name')
            ->where('age', '>', 18)
            ->orderBy('name', 'asc')
            ->get();
```

This query selects the "id" and "name" columns from the "users" table, applies a condition where the "age" column is greater than 18, and orders the results by the "name" column in ascending order.

## Inserting Data

To insert data into a table using the Query Builder, you can use the `insert` method. Here's an example:

```php
DB::table('users')->insert([
    'name' => 'John Doe',
    'email' => 'john@example.com',
    'password' => bcrypt('secret'),
]);
```

This query inserts a new record into the "users" table with the specified data.

## Updating Data

To update existing records using the Query Builder, you can use the `update` method. Here's an example:

```php
DB::table('users')
    ->where('id', 1)
    ->update(['name' => 'New Name', 'email' => 'new@example.com']);
```

This query updates the "name" and "email" columns of the user with ID 1.

## Deleting Data

To delete records from a table using the Query Builder, you can use the `delete` method. Here's an example:

```php
DB::table('users')->where('age', '<', 18)->delete();
```

This query deletes all records from the "users" table where the "age" column is less than 18.

## Querying Joins

The Query Builder supports various types of joins, including inner joins, left joins, and right joins. Here's an example of an inner join:

```php
$users = DB::table('users')
            ->join('orders', 'users.id', '=', 'orders.user_id')
            ->select('users.name', 'orders.order_number')
            ->get();
```

This query joins the "users" table with the "orders" table based on the "id" and "user_id" columns, and selects the "name" from the "users" table and the "order_number" from the "orders" table.
