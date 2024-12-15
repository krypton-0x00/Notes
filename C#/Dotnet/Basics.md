## **1. Basic Setup: "Express-like" Starter**

In **Express.js**, you'd write something like this to start a server:

```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.send('Hello, World!');
});

app.listen(3000, () => {
    console.log('Server is running on port 3000');
});
```

In **ASP.NET Core**, the equivalent looks like this:

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Http;

var builder = WebApplication.CreateBuilder(args); // Initialize the app
var app = builder.Build();                        // Build the app

app.MapGet("/", () => "Hello, World!");           // Define a route

app.Run();                                        // Start the server
```

- **Explanation**:
    - `WebApplication.CreateBuilder(args)`: Like `express()`, it sets up the app.
    - `app.MapGet("/", () => ...)`: Like `app.get('/', ...)`, it maps an HTTP GET route to a handler.
    - `app.Run()`: Starts the server.

---

## **2. Middleware**

In **Express.js**, middleware is a function that processes requests and responses:

```javascript
app.use((req, res, next) => {
    console.log(`${req.method} ${req.url}`);
    next(); // Pass control to the next middleware
});
```

In **ASP.NET Core**, middleware works similarly but uses a pipeline:

```csharp
app.Use(async (context, next) =>
{
    Console.WriteLine($"{context.Request.Method} {context.Request.Path}");
    await next.Invoke(); // Pass to the next middleware
});
```

- The `next` function in Express is like `next.Invoke()` in ASP.NET Core.

---

## **3. Routes**

### Express.js:

```javascript
app.get('/users', (req, res) => {
    res.send('User list');
});
```

### ASP.NET Core:

```csharp
app.MapGet("/users", async context =>
{
    await context.Response.WriteAsync("User list");
});
```

- Routes in ASP.NET Core are defined using methods like `MapGet` for GET requests, similar to `app.get` in Express.

---

## **4. Dependency Injection**

ASP.NET Core has **built-in dependency injection (DI)**, unlike Express.js, where you might manually require dependencies.

### Example in ASP.NET Core:

1. **Register a Service**:
    
    ```csharp
builder.Services.AddSingleton<IMyService, MyService>();
    ```
    
2. **Inject It in a Route**:
    
    ```csharp
    app.MapGet("/data", (IMyService myService) =>
    {
        return myService.GetData();
    });
    ```
    

---

## **5. Controllers (Advanced Routing)**

Express.js doesn’t have a strict concept of controllers, but you might organize your routes into separate files/modules.

### Express.js Example:

```javascript
const usersRouter = require('./routes/users');
app.use('/users', usersRouter);
```

### ASP.NET Core Equivalent:

ASP.NET Core uses **Controllers** for organizing routes:

1. **Define a Controller**:
    
    ```csharp
    using Microsoft.AspNetCore.Mvc;
    
    [ApiController]
    [Route("[controller]")]
    public class UsersController : ControllerBase
    {
        [HttpGet]
        public IActionResult GetUsers()
        {
            return Ok(new[] { "User1", "User2" });
        }
    }
    ```
    
2. **Enable Controllers**:
    
    ```csharp
    builder.Services.AddControllers();
    app.MapControllers();
    ```
    

---

## **6. JSON Responses**

In Express.js, you'd return JSON like this:

```javascript
app.get('/json', (req, res) => {
    res.json({ message: 'Hello, JSON!' });
});
```

In ASP.NET Core, it's similar:

```csharp
app.MapGet("/json", () => new { Message = "Hello, JSON!" });
```

---

## **7. Working with Static Files**

### Express.js:

```javascript
app.use(express.static('public'));
```

### ASP.NET Core:

```csharp
app.UseStaticFiles();
```

Place your static files (e.g., CSS, JS, images) in the `wwwroot` directory.

---

## **8. Database Connection**

### Express.js + MongoDB Example:

```javascript
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost:27017/mydb', { useNewUrlParser: true });

app.get('/data', async (req, res) => {
    const data = await MyModel.find();
    res.json(data);
});
```

### ASP.NET Core + Entity Framework (SQL Database):

1. **Install Entity Framework Core**:
    
    ```bash
    dotnet add package Microsoft.EntityFrameworkCore.SqlServer
    ```
    
2. **Define a DbContext**:
    
    ```csharp
    public class AppDbContext : DbContext
    {
        public DbSet<Product> Products { get; set; }
    }
    ```
    
3. **Use the DbContext in a Route**:
    
    ```csharp
    app.MapGet("/data", async (AppDbContext db) =>
    {
        return await db.Products.ToListAsync();
    });
    ```
    

---

## **9. Middleware for Errors**

### Express.js:

```javascript
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).send('Something broke!');
});
```

### ASP.NET Core:

```csharp
app.UseExceptionHandler("/error");
app.MapGet("/error", async context =>
{
    context.Response.StatusCode = 500;
    await context.Response.WriteAsync("Something broke!");
});
```

---

### **11. Configuration (Environment Variables and Settings)**

In **Express.js**, you might use **dotenv** to manage configuration:

```javascript
require('dotenv').config();
const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});
```

In **ASP.NET Core**, the configuration system is even more powerful:

1. **Add Configurations in `appsettings.json`**:
    
    ```json
    {
        "AppSettings": {
            "AppName": "My App",
            "Version": "1.0.0"
        }
    }
    ```
    
2. **Read Settings in Code**:
    
    ```csharp
    var config = builder.Configuration;
    var appName = config["AppSettings:AppName"];
    var version = config["AppSettings:Version"];
    Console.WriteLine($"Running {appName} v{version}");
    ```
    
3. **Use Environment Variables**: ASP.NET Core automatically picks up environment variables without extra libraries. For example:
    
    ```bash
    export ASPNETCORE_ENVIRONMENT=Development
    ```
    
    Then, access the environment:
    
    ```csharp
    var env = builder.Environment.EnvironmentName; // "Development" or "Production"
    ```
    

---

### **12. Dependency Injection (Deep Dive)**

Dependency Injection (DI) is a core feature in ASP.NET Core. If you’ve manually `require()`-ed things in Express.js, this will feel a bit magical but super useful.

#### **Example: A Simple Service**

Let’s say you want to log messages from your app.

1. **Create the Service**:
    
    ```csharp
    public interface ILoggerService
    {
        void Log(string message);
    }
    
    public class LoggerService : ILoggerService
    {
        public void Log(string message)
        {
            Console.WriteLine($"LOG: {message}");
        }
    }
    ```
    
2. **Register the Service**: In Express, you’d `require()` and use the service. In ASP.NET Core:
    
    ```csharp
    builder.Services.AddSingleton<ILoggerService, LoggerService>();
    ```
    
3. **Use the Service**: Inject the service where needed:
    
    ```csharp
    app.MapGet("/log", (ILoggerService logger) =>
    {
        logger.Log("Hello from ASP.NET Core!");
        return Results.Ok("Logged the message");
    });
    ```
    

This DI system ensures clean, testable, and maintainable code.

---

### **13. Middleware Pipeline (Request Flow)**

In Express.js, middleware is executed sequentially:

```javascript
app.use((req, res, next) => {
    console.log('Middleware 1');
    next();
});

app.use((req, res, next) => {
    console.log('Middleware 2');
    next();
});

app.get('/', (req, res) => {
    res.send('Hello, World!');
});
```

In ASP.NET Core, the middleware pipeline works the same way, but with some differences in syntax:

```csharp
app.Use(async (context, next) =>
{
    Console.WriteLine("Middleware 1");
    await next.Invoke();
});

app.Use(async (context, next) =>
{
    Console.WriteLine("Middleware 2");
    await next.Invoke();
});

app.MapGet("/", () => "Hello, World!");
```

- Middleware components are processed in the order they’re added.
- Use `await next.Invoke()` to call the next middleware in the pipeline.

---

### **14. Authentication and Authorization**

In Express.js, you might use **passport.js** for authentication:

```javascript
const passport = require('passport');
app.use(passport.initialize());
```

In ASP.NET Core, authentication and authorization are built-in.

#### **Example: Adding Basic Authentication**

1. **Install the NuGet Package**:
    
    ```bash
    dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer
    ```
    
2. **Configure Authentication**:
    
    ```csharp
    builder.Services.AddAuthentication("Bearer")
        .AddJwtBearer(options =>
        {
            options.Authority = "https://your-auth-provider";
            options.Audience = "your-api";
        });
    ```
    
3. **Protect Routes**:
    
    ```csharp
    app.UseAuthentication();
    app.UseAuthorization();
    
    app.MapGet("/secure", [Authorize] () => "This is a secure endpoint");
    ```
    

---

### **15. Entity Framework Core (Database ORM)**

Entity Framework Core (EF Core) is like Mongoose but for relational databases like SQL Server, PostgreSQL, or SQLite.

#### **Example: Setup and Query a Database**

1. **Install EF Core Packages**:
    
    ```bash
    dotnet add package Microsoft.EntityFrameworkCore.SqlServer
    ```
    
2. **Define a Model**:
    
    ```csharp
    public class Product
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public decimal Price { get; set; }
    }
    ```
    
3. **Setup DbContext**:
    
    ```csharp
    public class AppDbContext : DbContext
    {
        public DbSet<Product> Products { get; set; }
    
        protected override void OnConfiguring(DbContextOptionsBuilder options)
        {
            options.UseSqlServer("YourConnectionString");
        }
    }
    ```
    
4. **Query Data in a Route**:
    
    ```csharp
    app.MapGet("/products", async (AppDbContext db) =>
    {
        return await db.Products.ToListAsync();
    });
    ```
    

---

### **16. Building a REST API**

Let’s put everything together to build a simple **CRUD API**.

#### **Step 1: Models**

```csharp
public class Todo
{
    public int Id { get; set; }
    public string Title { get; set; }
    public bool IsComplete { get; set; }
}
```

#### **Step 2: DbContext**

```csharp
public class AppDbContext : DbContext
{
    public DbSet<Todo> Todos { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder options)
    {
        options.UseSqlite("Data Source=todos.db");
    }
}
```

#### **Step 3: Add Endpoints**

```csharp
app.MapGet("/todos", async (AppDbContext db) => await db.Todos.ToListAsync());

app.MapPost("/todos", async (AppDbContext db, Todo todo) =>
{
    db.Todos.Add(todo);
    await db.SaveChangesAsync();
    return Results.Created($"/todos/{todo.Id}", todo);
});

app.MapPut("/todos/{id}", async (AppDbContext db, int id, Todo input) =>
{
    var todo = await db.Todos.FindAsync(id);
    if (todo is null) return Results.NotFound();

    todo.Title = input.Title;
    todo.IsComplete = input.IsComplete;
    await db.SaveChangesAsync();

    return Results.NoContent();
});

app.MapDelete("/todos/{id}", async (AppDbContext db, int id) =>
{
    var todo = await db.Todos.FindAsync(id);
    if (todo is null) return Results.NotFound();

    db.Todos.Remove(todo);
    await db.SaveChangesAsync();

    return Results.NoContent();
});
```

#### **Step 4: Run Migrations**

1. Install EF Core CLI:
    
    ```bash
    dotnet tool install --global dotnet-ef
    ```
    
2. Add a Migration and Update Database:
    
    ```bash
    dotnet ef migrations add InitialCreate
    dotnet ef database update
    ```
    

---

 