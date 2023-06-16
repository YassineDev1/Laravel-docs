# Database Migrations

Migrations in Laravel provide a convenient way to manage and version your database schema. They allow you to create and modify database tables, indexes, and foreign keys in a structured and organized manner. Laravel's migration system makes it easy to collaborate with other developers and keep your database schema in sync across different environments. In this README, we will cover the basics of using migrations in Laravel.

## Table of Contents
- [Introduction to Migrations](#introduction-to-migrations)
- [Creating Migrations](#creating-migrations)
- [Migration Structure](#migration-structure)
- [Running Migrations](#running-migrations)
- [Rolling Back Migrations](#rolling-back-migrations)
- [Migration Status](#migration-status)
- [Migration Seeds](#migration-seeds)

## Introduction to Migrations

Migrations allow you to define the structure of your database using PHP code. Each migration represents a set of instructions to create or modify database tables, columns, indexes, and other schema elements. Laravel's migration system tracks which migrations have been executed, making it easy to migrate your database schema across different environments and collaborate with other developers.

## Creating Migrations

To create a new migration, you can use the `make:migration` Artisan command:

```shell
php artisan make:migration create_users_table
```

This will generate a new migration file in the `database/migrations` directory. The migration file's name includes a timestamp prefix to ensure the order in which migrations are executed.

Open the newly created migration file and locate the `up` method. This method contains the instructions for creating the desired database schema. You can use Laravel's Schema Builder to define tables, columns, indexes, and other schema elements.

```php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateUsersTable extends Migration
{
    public function up()
    {
        Schema::create('users', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->string('email')->unique();
            $table->timestamp('email_verified_at')->nullable();
            $table->string('password');
            $table->rememberToken();
            $table->timestamps();
        });
    }

    public function down()
    {
        Schema::dropIfExists('users');
    }
}
```

In the example above, we define a migration to create a `users` table with various columns such as `name`, `email`, `password`, and timestamps for record creation and updates. The `up` method is responsible for creating the table, while the `down` method defines how to roll back the migration (i.e., drop the `users` table).

You can add additional methods to the migration class as needed to perform specific database schema modifications or data manipulations.

## Running Migrations

To run the pending migrations and update your database schema, you can use the `migrate` Artisan command:

```shell
php artisan migrate
```

This command will execute all pending migrations that haven't been run yet. Laravel tracks which migrations have been executed by storing their names in the database's `migrations` table.

By default, the `migrate` command runs all pending migrations. If you want to run migrations from a specific path, you can use the `--path` option:

```shell
php artisan migrate --path=database/migrations/custom
```

In the example above, the `migrate` command will only execute migrations from the `database/migrations/custom` directory.

## Rolling Back Migrations

To roll back the last batch of migrations, you can use the `migrate:rollback` Artisan command:



```shell
php artisan migrate:rollback
```

This command will undo the last set of migrations by calling the `down` method of each migration in reverse order.

If you want to rollback a specific number of migrations, you can use the `--step` option:

```shell
php artisan migrate:rollback --step=2
```

In the example above, the `migrate:rollback` command will roll back the last two batches of migrations.

## Migration Status

To view the status of your migrations, you can use the `migrate:status` Artisan command:

```shell
php artisan migrate:status
```

This command will display a table showing the status of each migration, indicating if it has been run or not.

## Migration Seeds

Migration seeds allow you to populate your database with dummy or default data. Seeds are often used to initialize database tables with sample data for development or testing purposes. Laravel provides a convenient way to generate seed classes using the `make:seeder` Artisan command:

```shell
php artisan make:seeder UsersTableSeeder
```

This will generate a new seeder class in the `database/seeders` directory. Open the generated seeder class and define the `run` method. This method contains the code to insert data into the database.

```php
use Illuminate\Database\Seeder;
use Illuminate\Support\Facades\DB;

class UsersTableSeeder extends Seeder
{
    public function run()
    {
        DB::table('users')->insert([
            'name' => 'John Doe',
            'email' => 'john@example.com',
            'password' => bcrypt('secret'),
        ]);
    }
}
```

In the example above, we define a seeder to insert a single user into the `users` table.

To run the seeders and populate your database, you can use the `db:seed` Artisan command:

```shell
php artisan db:seed
```

By default, this command runs all seeders defined in the `database/seeders` directory. If you want to run a specific seeder class, you can use the `--class` option:

```shell
php artisan db:seed --class=UsersTableSeeder
```

In the example above, only the `UsersTableSeeder` seeder will be executed.
