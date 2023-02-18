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
php artisan make:migration create_contacts_table
```

this will generate a file in database\migrations folder.

The file consist of new class extending the migration class of LARAVEL.

The new class consist of 2 major function up() & down().
The up() function holds all information about migrating the file

```
public function up()
{
    Schema::create('contacts', function (Blueprint $table) 
    {
            $table->id();
            $table->string('name');
            $table->string('mobile_no');
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

For rolling back latest migration we have command
```
php artisan migrate:rollback
```

when we have to rollback till specific steps we can pass steps in rollback command like
```
php artisan migrate:rollback --step=3
```
this will rollback the migration upto 3 step starting from latest.




