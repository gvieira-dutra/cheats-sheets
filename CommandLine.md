### Create New Project

```bash
dotnet new [Project Type] [project name]
```

- some project types:
  - `console` - Console application
  - `webapi` - Web API application
  - `mvc` - ASP.NET MVC application
  - `classlib` - Class library
  - `blazorserver` - Blazor Server application
  - `blazorwasm` - Blazor WebAssembly application

---

### Build Application

```bash
dotnet build
```

---

### Run Application

```bash
dotnet run
```

---

### Restore Dependencies

```bash
dotnet restore
```

- Restores dependencies and project-specific tools.

---

### Add Package

```bash
dotnet add package [PackageName]
```

- Adds a NuGet package to the project.
- https://nuget.org/

---

### Add Reference

```bash
dotnet add reference [path-to-project.csproj]
```

- Adds a reference to another project in the solution.

---

### Remove Package

```bash
dotnet remove package [PackageName]
```

---

### Test Application

```bash
dotnet test
```

---

### Publish Application

```bash
dotnet publish -c Release -o [output-folder]
```

- Publishes the application in the specified configuration (e.g., Release).

---

### Create Solution

```bash
dotnet new sln -n [SolutionName]
```

---

### Add Project to Solution

```bash
dotnet sln add [path-to-project.csproj]
```

---

### List Installed Templates

```bash
dotnet new --list
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

### Clean Project

```bash
dotnet clean
```

---

### Remove Project from Solution

```bash
dotnet sln remove [path-to-project.csproj]
```

### Checking & Manipulating Certificates

```
dotnet dev-certs https --check --trust [checks]
dotnet dev-certs https --clean [delete]
dotnet dev-certs https [creates one]
dotnet dev-certs https --trust [makes certs thrusted]

```

### Add and Drop Migrations

change `add` to `drop` in the commands below to drop a database.

```
dotnet ef migrations add [migration Name]
```

if there's more than one context:

```
dotnet ef migrations add [migration Name] --context [context name]
```
