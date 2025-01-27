# FIRST STEPS

Create a project  
Create entity classes  
Create ApplicationContext class and configure connection string

## DATABASE

### CREATE

Below are 4 different ways we can use to insert single objects on database. .SaveChanges() will return the number of state entries written to db.

```
        db.Products.Add(product);
        db.Set<Product>().Add(product2);
        db.Entry(product3).State = EntityState.Added;

        db.Add(product4);
        var records = db.SaveChanges();
```

To insert an array of objs, below can be used:

```
        db.Clients.AddRange(clientes);
```

### READ

We can read by LINK method:

```
         var clients = db.Clients.Where(p => p.Id > 0).ToList();
```

Or read by Syntax

```
        var clients = (from c in db.Clients where c.Id > 0 select c).ToList();
```

### UPDATE

Disconnected Approach:
Good way to perform an update is to instantiate an obj of the type to be updated with the id only, then, create an obj with the properties to be updated. Next we attach the obj to the database so it can track it and we set the values.

This approach is advantageous in comparison to the .Insert method because it only updates the properties that are being updated, whereas the .Insert updates all properties, even the ones that are not changing are updated to themselves.

Example below:

```
        using var db = new Data.ApplicationContext();

        var client = new Client
        {
            Id = 1
        };

        var disconnectedClient = new
        {
            Name = "EDITED NAME"
        };

        db.Attach(client);
        db.Entry(client).CurrentValues.SetValues(disconnectedClient);

        db.SaveChanges();
```

### DELETE

Somewhat similar approach can be used to delete and obj.

```
        var client = new Client { Id = 1 };
        db.Entry(client).State = EntityState.Deleted;
```

### Ensure Created - Deleted

Ensure Created is an alternative for migration  
Ensure Deleted is used to delete everything from the database

### Ensure Created For Multiple Entities

In this approach, we first create the database based on the db1 instance, then we create a table or db2 as the database already exists!!

To create multiple tables coming from different application contexts, an approach like the below is needed:

```
private static void GapEnsureCreated()
{
using var db1 = new Data.ApplicationContextCidade();
using var db2 = new Data.ApplicationContext();

     db1.Database.EnsureCreated();

     var dbCreator = db2.GetService<IRelationalDatabaseCreator>();

     dbCreator.CreateTables();

}
```

### DATABASE Health Check

Checking the db’s health will ensure that our application is able to connect to the database and allow us to troubleshoot any problems at an early stage.

```
private static void DbHealthCheck()
{
using var db = new Data.ApplicationContext();
try
{
var connection = db.Database.GetDbConnection();
connection.Open();

         Console.WriteLine("CONNECTION SUCCESS");
     }
     catch
     {
         Console.WriteLine("CONNECTION FAILURE");
     }

}
```

OR

```
if (db.Database.CanConnect())
{
    Console.WriteLine("CONEXION SUCCESS");
}
else
{
    Console.WriteLine("CONEXION FAIL");
}
```

### Managing Connection State

Whenever a method needs to perform several interactions with database, we should manage the connection ourselves and keep it open for the entire duration of all operations.
Can be achieved with the code below:

```
private static void FunctionWithConnectionStateManagement(bool manageState)
{
    using var db = new Data.ApplicationContext();
    var time = System.Diagnostics.Stopwatch.StartNew();

    var connection = db.Database.GetDbConnection();
    connection.StateChange += (_, __) => ++_count;

    if (manageState)
    {
        connection.Open();
    }

    for (var i = 0; i < 200; i++)
    {
        db.Departments.AsNoTracking().Any(); // multiple db access
    }
    time.Stop();

    var msg = $"Tempo: {time.Elapsed.ToString()} -- {manageState} -- State Change Cnt: {_count}";

    Console.WriteLine(msg);
}
```

### Executing SQL Directly

Below exemplifies how we can directly use SQL commands against database

```
    private static void ExecuteSQL()
    {
        using var db = new Data.ApplicationContext();

        //Option 1
        using var cmd = db.Database.GetDbConnection().CreateCommand();
        cmd.CommandText = "SELECT...";
        cmd.ExecuteNonQuery();

        //Option 2
        var description = "test";
        db.Database.ExecuteSqlRaw(@"SELECT *
                                    FROM Table
                                    WHERE Description={0}",
                                    description);

        //Option 3
        db.Database.ExecuteSqlInterpolated(@$"SELECT *
                                              FROM Table
                                              WHERE Description={description}");

    }
```

### Protecting Against SQL Injection

Always user parameters, NEVER USE STRING INTERPOLATION

### Detecting Migrations

With the code below, we are able to retrieve how many pending migrations our application has:

```
using var db = new Curso.Data.ApplicationContext();

var pendingMigrations = db.Database.GetPendingMigrations();
```

To retrieve ALL MIGRATIONS, we can use `.GetMigrations()` instead.

To retrieve a list of APPLIED migrations we can use `.GetAppliedMigrations()` instead.

To list all migrations, applied and pending, we can:

`dotnet ef migrations list --context`

To list applied migrations we can:

### Applying Migration on Execution Time

NOT A GOOD PRACTICE, BUT POSSIBLE

The command below when called on execution time will simple apply the migrations on the database.

```
using var db = new Curso.Data.ApplicationContext();

db.Database.Migrate();
```

### Generating DB Script

To generate a script so we can see what exactly is being done on our database, we can:

```
using var db = new ApplicationContext();

var script = db.Database.GenerateCreateScript();
```
