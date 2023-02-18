## Migrations

Laravel Migration is an essential feature in Laravel that allows you to create a table in your database.
It allows you to modify and share the application's database schema.
You can modify the table by adding a new column or deleting an existing column.

Migrations are like version control for your database, allowing your team to define and share the application's database schema definition. If you have ever had to tell a teammate to manually add a column to their local database schema after pulling in your changes from source control, you've faced the problem that database migrations solve.

### Why do we need Laravel Migration?

Suppose we are working in a team, and some idea strikes that require the alteration in a table.
In such a case, the SQL file needs to be passed around, and some team member has to import that file, but team member forgot to import the SQL file.
In this case, the application will not work properly, to avoid such situation, Laravel Migration comes into existence.

Laravel Migration allows you to add a new column or delete the records in your database without deleting the records that are already present.


### Create a migration
```
php artisan make:migration create_contacts_table    // this will generate migration.
or 
php artisan make:model YourModelName -c -m      // this will generate model along with controller and migration.
```

this will generate a file in database\migrations folder.

Go to /database/migrations/xxxx_xx_xx_xxxxxx_create_contacts_table.php

All the migration file that we create using the artisan command are located at database/migrations directory. So, after we run the above command, it will generate a PHP file with the name we specified with the current date and time.

The file consist of new class extending the migration class of LARAVEL.

The new class consist of 2 major function up() & down().

The up() function holds all information about migrating the file

#### Basic Structure of a Migration:

A migration file contains a class with the name of the file specified while creating the migration and it extends Migration. In that we have two functions, the first one is up() function and the second one is down() function. The up() is called when we run the migration to create a table and columns specified and the ‘down()’ function is called when we want to undo the creation of ‘up()’ function.

In the up() function, we have create method of the Schema facade (schema builder) and as we say before, the first argument in this function is the name of the table to be created. The second argument is a function with a Blueprint object as a parameter and for defining the table.

In the down() function, we have a dropIfExists method of the schema builder which when called will drop the table.

```
public function up()
{
    Schema::create('contacts', function (Blueprint $table) 
    {
            $table->id();   // Each user has an id
            $table->string('name');     // Each user has a name
            $table->string('mobile_no');
            $table->string('email')->unique();        // Each user has a unique email
            $table->boolean('status');
            $table->timestamps();
    });
}
```

whereas down() function holds information about reversing the migration action.

```
public function down()
{
    Schema::dropIfExists('contacts');
}
```

### Run & Rollback The Migration
To run a migration we need to use the command
```
php artisan migrate
```
This command will run the up() function in the migration class file and will create the table with the all columns specified.

Note: This command will run the up() function and create all the tables in the database for all the migration file in the database/migrations directory.

#### rollback

For rolling back latest migration we have command
```
php artisan migrate:rollback
```
This command will run the down() function in the migration class file.

when we have to rollback till specific steps we can pass steps in rollback command like
```
php artisan migrate:rollback --step=3
```
this will rollback the migration upto 3 step starting from latest.

#### reset
The migrate:reset command will roll back all of your application's migrations:
```
php artisan migrate:reset
```

#### The migrate:reset command will roll back all of your application's migrations:

If you would like to see which migrations have run thus far, you may use the migrate:status Artisan command:
```
php artisan migrate:status
```

#### To see the SQL statments executed during migrations.

If you would like to see the SQL statements that will be executed by the migrations without actually running them, you may provide the --pretend flag to the migrate command:

```
php artisan migrate --pretend
```




