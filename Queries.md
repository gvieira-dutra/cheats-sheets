## Global Filter Configuration

Global filter wil apply a filter to all queries performed on database. For example in logical deletion where you don't want to add the deleted check to all queries. Can be done as:

```
protected override void OnModelCreating(ModelBuilder modelBuilder){
    modelBuilder.Entity<Entity>().HasQueryFilter(p => !Active);
    //Where Active is a boolean to represent if the record is or not active.
}
```

<strong>To ignore a global filter in a query</strong> we can:

```
db.Department.IgnoreQueryFilters().Where...
```

## Projection

Strategy to only retrieve needed info from database, reducing data traffic. Example:

```
var department = db.Department
    .Where(p => p.Id > 0)
    .Select(p => new {p.Descrip, Employees = p.Employees.Select(e => e.Name)})
    .ToList();
```

The above allows us to recover department description and employee names only from db, instead of all data.

## Parameterized Queries

When LINQ might not be enough to reflect the complexity of our queries, we can use SQL to execute a query on db, such as

```
var id = new SqlParameter
{
    Value = 1,
    SqlDbType = System.Data.SqlDbType.Int
};

db.Department.SqlRaw("SELECT \*
                      FROM Department
                      WHERE Id > {0}", id)
              .ToList();
```

<strong> Interpolated Queries </strong>

```
var id = 1;

db.Department.FromSqlInterpolated($"SELECT \*
                                    FROM Department
                                  WHERE Id > {id}")
             .ToList();
```

## Query TAGs

Used to send additional info to database

```
var department = db.Departments
                   .TagWith("Sending comment to server")
                   .ToList();
```

## 1xN vs Nx1 Queries

<strong> Scenario 1: </strong>Query below will retrieve all departments, then all employees that are part of retrieved department.

```
var department = db.Departments
    .Include(p => p.Employees)
    .ToList();
```

<strong> Scenario 2: </strong>Query below will retrieve all employees, then all departments that they belong to.

```
var department = db.Employees
    .Include(p => p.Departments)
    .ToList();
```

## SplitQuery

Made to solve Cartesian Explosion issue, where duplicate data can exponentially increase the overall transferred data.

In the code below, duplicate data from Departments will be brought from db, where for each Employee, all info about a Department will be brought alongside.

```
var departments = db.Departments
    .Include(p => p.Employees)
    .Where(p => p.Id > 0)
    .ToList();
```

To mitigate this issues we can:

```
var departments = db.Departments
    .Include(p => p.Employees)
    .Where(p => p.Id > 0)
    .AsSplitQuery()
    .ToList();
```

The above query, will generate 2 queries, one that retrieves all Departments and a second that retrieves all Employees for each department.

Can be applied globally with the string connection configurations by adding to the db config `UseSqlServer(strConnection, p => p.UserQuerySplittingBehavior(QuerySplittingBehavior.SplitQuery))`

To ignore global split query in a query, we can add `AsSingleQuery()` to a specific query.
