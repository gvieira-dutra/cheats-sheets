## Advanced Loading Types

### Lazy Loading

Data is only loaded by demand when property is accessed

Can be achieved with the package `EntityFrameworkCore.Proxies` and adding to the connection string the property `.UseLazyLoadingProxies=true`. Then we make the navigation property as `virtual` in our entity.

Code example:

```
            var department = db
                .departments
                .ToList();

            foreach (var department in departments)
            {
                Console.WriteLine("---------------------------------------");
                Console.WriteLine($"Department: {department.Descrip}");

                if (department.Employee?.Any() ?? false)
                {
                    foreach (var employee in department.Employee)
                    {
                        Console.WriteLine($"\tEmployee: {employee.Nome}");
                    }
                }
                else
                {
                    Console.WriteLine($"\No employee found!");
                }
            }
        }
```

<strong>CONSIDERATION: </strong> Each query will create an access to database so if there are many queries to be made, this will slow down application as it will connect once to each query.

We can also disable lazy loading for a specific method by adding the following line of code at the beginning of the method, `db.ChangeTracker.LazyLoadingEnable = false`

For lazy loading without proxies, as can code on the entity a private constructor that takes an injection of `ILazyLoader` and uses an instance of it to load the contained class. Code below:

```
private ILazyLoader _lazyLoader {get;set;}
private Department(ILazyLoader lazyLoader)
{
    _lazyLoader = lazyLoader;
}

private List<Employee> _employees;
public List<Employee> Employees
{
    get => _lazyLoader.Load(this, ref _employees);
    set => _employees = value;
}
```

Lastly, we can pass an `Action` to implement lazy loading. Such as:

```
private Action<object,string> _lazyLoader {get;set;}
private Department(Action<object,string> lazyLoader)
{
    _lazyLoader = lazyLoader;
}

private List<Employee> _employees;
public List<Employee> Employees;
{
    get
    {
        _lazyLoader?.Invoke(this, nameof(Employee));

        return _employees;
    }
    set => _employees = value;
}
```

### Eager Loading

Eager loading can be applied using a `.Include()` to include entities that belongs to another entity and load them all. For example:

```
var departments = db.
    Departments
    .Include(e => e.employee);
```

Method above will retrieve all departments along with all employees that are part of this department.

DISADVANTAGE: Cartesian explosion. This is when the query returns exponentially more data than needed. For example if there was 5k employees in a department, the department info would be retrieved 5k times. Gets worse the more fields are being retrieved from department.

### Explicit Loading

Data is only loaded when requested. Code can look like:

```
var departments = db.
    Departments
    .ToList();


foreach (var department in departaments)
{
    if(department.Id == 2)
    {
        //db.Entry(department).Collection(p=>p.Employee).Load();
        db.Entry(department).Collection(p=>p.Employee).Query().Where(p=>p.Id > 2).ToList();
    }

    Console.WriteLine("---------------------------------------");
    Console.WriteLine($"department: {department.Descrip}");

    if (department.Employee?.Any() ?? false)
    {
        foreach (var employee in department.Employees)
        {
            Console.WriteLine($"\tEmployee: {employee.Nome}");
        }
    }
    else
    {
        Console.WriteLine($"\tNo employee found!");
    }
}

```

<strong>pass to collection the navigation property we want data to be filled for</strong>

<strong> The .ToList(); is used to prevent the error </strong> `There is already an open DataReader associated with this Connection which must be closed first` <strong> Another alternative is to add the following parameter to the connection string </strong> `MultipleActiveResultSets=true`
