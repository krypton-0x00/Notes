To manually add Entity Framework to your project in **Visual Studio Code (VS Code)**, you can use the **NuGet CLI** or a `.csproj` file to manage dependencies without relying on the NuGet Gallery GUI. Here's how you can set it up step-by-step:

---

## **Steps to Add Entity Framework Manually**

### **1. Ensure .NET is Installed**

Make sure you have the .NET SDK installed. You can check by running:

```bash
dotnet --version
```

If not installed, download it from the [.NET website](https://dotnet.microsoft.com/download).

---

### **2. Create a New Project (if needed)**

If you don’t have a project already, create one:

```bash
dotnet new console -n MyProject
cd MyProject
```

This will create a new Console Application with a `.csproj` file.

---

### **3. Add Entity Framework Using `dotnet add package`**

You can add Entity Framework Core packages manually using the `dotnet add package` command.

#### Example:

To add the main Entity Framework Core package:

```bash
dotnet add package Microsoft.EntityFrameworkCore
```

To add a database provider (e.g., SQL Server or SQLite), use:

- For SQL Server:
    
    ```bash
    dotnet add package Microsoft.EntityFrameworkCore.SqlServer
    ```
    
- For SQLite:
    
    ```bash
    dotnet add package Microsoft.EntityFrameworkCore.Sqlite
    ```
    
- For MySQL:
    
    ```bash
    dotnet add package Pomelo.EntityFrameworkCore.MySql
    ```
    

To add the Entity Framework Core tools (for migrations):

```bash
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

---

### **4. Modify the `.csproj` File (Manual Approach)**

Alternatively, you can directly edit the `.csproj` file to include the necessary packages.

#### Example `.csproj`:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net6.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore" Version="7.0.0" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer" Version="7.0.0" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="7.0.0" />
  </ItemGroup>

</Project>
```

To update the project, run:

```bash
dotnet restore
```

---

### **5. Verify Installation**

Check installed packages by running:

```bash
dotnet list package
```

You should see the Entity Framework packages you added.

---

### **6. Scaffold a Database Context (Optional)**

If you want to scaffold a database or create migrations, make sure you have the tools package installed:

```bash
dotnet add package Microsoft.EntityFrameworkCore.Design
```

#### To Scaffold a Database:

```bash
dotnet ef dbcontext scaffold "YourConnectionString" Microsoft.EntityFrameworkCore.SqlServer
```

#### To Add Migrations:

```bash
dotnet ef migrations add InitialMigration
dotnet ef database update
```

---

### **7. Start Using Entity Framework**

Now, you can write your `DbContext` and models in your project:

#### Example `DbContext`:

```csharp
using Microsoft.EntityFrameworkCore;

public class AppDbContext : DbContext
{
    public DbSet<User> Users { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer("YourConnectionString");
    }
}

public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```

---

### **8. Use Entity Framework Commands**

Run the following to ensure Entity Framework is set up properly:

- To add migrations:
    
    ```bash
    dotnet ef migrations add InitialMigration
    ```
    
- To apply migrations:
    
    ```bash
    dotnet ef database update
    ```
    

---

### **Conclusion**

By using the NuGet CLI and editing the `.csproj` file, you can easily add and manage Entity Framework in VS Code. Let me know if you need help with scaffolding models, writing queries, or setting up migrations!