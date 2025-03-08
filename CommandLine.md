### Projects

##### Create new

```bash
dotnet new [Project Type] -n [project name]
```

- some project types:
  - `console` - Console application
  - `webapi` - Web API application
  - `mvc` - ASP.NET MVC application
  - `classlib` - Class library
  - `blazorserver` - Blazor Server application
  - `blazorwasm` - Blazor WebAssembly application

---

##### Add Reference

```bash
dotnet add reference [path-to-project.csproj]
```

---

---

##### List Installed Templates

```bash
dotnet new --list
```

- Adds a reference to another project in the solution.

---

### Build, Clear, Run, etc

##### Build Application

```bash
dotnet build
```

---

##### Run Application

```bash
dotnet run
```

---

##### Clean Project

```bash
dotnet clean
```

---

### Test Application

```bash
dotnet test
```

---

### Restore Dependencies

```bash
dotnet restore
```

- Restores dependencies and project-specific tools.

---

### Packages

##### Add Package

```bash
dotnet add package [PackageName]
```

- Adds a NuGet package to the project.
- https://nuget.org/

---

##### Remove Package

```bash
dotnet remove package [PackageName]
```

---

### Publish Application

```bash
dotnet publish -c Release -o [output-folder]
```

- Publishes the application in the specified configuration (e.g., Release).

---

### Solutions

##### Create Solution

```bash
dotnet new sln -n [SolutionName]
```

##### Add Project to Solution

```bash
dotnet sln add [path-to-project.csproj]
```

##### Remove Project from Solution

```bash
dotnet sln remove [path-to-project.csproj]
```

---

### Update .NET SDK or Runtime

```bash
dotnet --list-sdks
dotnet --list-runtimes
```

- Lists installed SDKs or runtimes.

---

### Check .NET Version

```bash
dotnet --version
```

- Displays the installed .NET SDK version.

---

---

### Checking & Manipulating Certificates

```
dotnet dev-certs https --check --trust [checks]
dotnet dev-certs https --clean [delete]
dotnet dev-certs https [creates one]
dotnet dev-certs https --trust [makes certs thrusted]

```

### Managing Migrations

- change `add` to `drop` in the commands below to drop a database.

##### Adding or drop migration

```
dotnet ef migrations add [migration Name]
```

##### To update database

```
dotnet ef database update [optional migration name]
```

##### To remove last migration

```
dotnet ef migrations remove
```

##### To list all migrations

```
dotnet ef migrations list
```

##### To script the migration

```
dotnet ef migrations script
```

##### Revert database to specific migration

```
dotnet ef database update [MigrationName]

```

- if there's more than one context:

```
dotnet ef migrations add [migration Name] --context [context name]
```

### User Secrets

To add a user secrets resource on our project:
`dotnet user-secrets init`

To insert a secret:
`dotnet user-secrets set "DefaultConnection" "Server=localhost,1433;Database=my_finance-dev;User ID=sa;Password=Welcome123!;Trusted_Connection=False; TrustServerCertificate=True;"`

To remove all secrets:
`dotnet user-secrets clear`

### Windows Powershell

###### Find task running on a specific port

`netstat -ano | findstr :<PORT>`

###### Killing the task

`taskkill /PID <PID> /F`
