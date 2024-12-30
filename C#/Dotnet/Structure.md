If your project name is **Tournament**, here's how to structure and implement it with the name incorporated appropriately.

---

## **Updated Project Structure**

```
Tournament/
│
├── src/                          # Source code for the service
│   ├── Tournament.Api/           # Main API
│   ├── Tournament.Core/          # Core logic (entities, interfaces, services)
│   ├── Tournament.Data/          # Data access (EF Core, repositories, migrations)
│   ├── Tournament.Tests/         # Unit and integration tests
│
├── docs/                         # Documentation
├── tools/                        # Utility scripts
├── build/                        # Build or CI/CD pipeline scripts
├── .editorconfig                 # Coding style configurations
├── .gitignore                    # Files to ignore in Git
├── Directory.Build.props         # Common MSBuild properties
├── global.json                   # Specifies the .NET SDK version
└── README.md                     # Readme file
```

---

## **Implementation**

### **Step 1: Create the Solution and Projects**

1. Navigate to the folder and create the solution:
    
    ```bash
    mkdir Tournament
    cd Tournament
    dotnet new sln -n Tournament
    ```
    
2. Create projects:
    
    ```bash
    dotnet new webapi -n Tournament.Api
    dotnet new classlib -n Tournament.Core
    dotnet new classlib -n Tournament.Data
    dotnet new xunit -n Tournament.Tests
    ```
    
3. Add projects to the solution:
    
    ```bash
    dotnet sln add Tournament.Api/Tournament.Api.csproj
    dotnet sln add Tournament.Core/Tournament.Core.csproj
    dotnet sln add Tournament.Data/Tournament.Data.csproj
    dotnet sln add Tournament.Tests/Tournament.Tests.csproj
    ```
    
4. Set dependencies between projects:
    
    ```bash
    dotnet add Tournament.Api/Tournament.Api.csproj reference Tournament.Core/Tournament.Core.csproj
    dotnet add Tournament.Api/Tournament.Api.csproj reference Tournament.Data/Tournament.Data.csproj
    dotnet add Tournament.Data/Tournament.Data.csproj reference Tournament.Core/Tournament.Core.csproj
    ```
    

---

### **Step 2: Core Project**

#### **Domain Models**

Define your **Tournament** and **Player** models in the `Tournament.Core` project.

`Tournament.Core/Entities/Tournament.cs`:

```csharp
namespace Tournament.Core.Entities;

public class Tournament
{
    public Guid Id { get; set; }
    public string Name { get; set; } = string.Empty;
    public DateTime StartDate { get; set; }
    public DateTime EndDate { get; set; }
    public int MaxPlayers { get; set; }
    public List<Player> Players { get; set; } = new();
}
```

`Tournament.Core/Entities/Player.cs`:

```csharp
namespace Tournament.Core.Entities;

public class Player
{
    public Guid Id { get; set; }
    public string Name { get; set; } = string.Empty;
    public string Email { get; set; } = string.Empty;
}
```

#### **Interfaces**

Define contracts for repositories and services.

`Tournament.Core/Interfaces/ITournamentRepository.cs`:

```csharp
using Tournament.Core.Entities;

namespace Tournament.Core.Interfaces;

public interface ITournamentRepository
{
    Task<Tournament?> GetByIdAsync(Guid id);
    Task<IEnumerable<Tournament>> GetAllAsync();
    Task AddAsync(Tournament tournament);
    Task UpdateAsync(Tournament tournament);
    Task DeleteAsync(Guid id);
}
```

`Tournament.Core/Interfaces/ITournamentService.cs`:

```csharp
using Tournament.Core.Entities;

namespace Tournament.Core.Interfaces;

public interface ITournamentService
{
    Task<IEnumerable<Tournament>> GetAllTournamentsAsync();
    Task<Tournament?> GetTournamentByIdAsync(Guid id);
    Task CreateTournamentAsync(Tournament tournament);
    Task AddPlayerToTournamentAsync(Guid tournamentId, Player player);
    Task DeleteTournamentAsync(Guid id);
}
```

---

### **Step 3: Data Project**

#### **DbContext**

Create a database context in `Tournament.Data`.

`Tournament.Data/TournamentDbContext.cs`:

```csharp
using Microsoft.EntityFrameworkCore;
using Tournament.Core.Entities;

namespace Tournament.Data;

public class TournamentDbContext : DbContext
{
    public TournamentDbContext(DbContextOptions<TournamentDbContext> options) : base(options) { }

    public DbSet<Tournament> Tournaments { get; set; }
    public DbSet<Player> Players { get; set; }
}
```

#### **Repository Implementation**

Implement the `ITournamentRepository` interface.

`Tournament.Data/Repositories/TournamentRepository.cs`:

```csharp
using Microsoft.EntityFrameworkCore;
using Tournament.Core.Entities;
using Tournament.Core.Interfaces;

namespace Tournament.Data.Repositories;

public class TournamentRepository : ITournamentRepository
{
    private readonly TournamentDbContext _context;

    public TournamentRepository(TournamentDbContext context)
    {
        _context = context;
    }

    public async Task<Tournament?> GetByIdAsync(Guid id)
    {
        return await _context.Tournaments.Include(t => t.Players).FirstOrDefaultAsync(t => t.Id == id);
    }

    public async Task<IEnumerable<Tournament>> GetAllAsync()
    {
        return await _context.Tournaments.Include(t => t.Players).ToListAsync();
    }

    public async Task AddAsync(Tournament tournament)
    {
        await _context.Tournaments.AddAsync(tournament);
        await _context.SaveChangesAsync();
    }

    public async Task UpdateAsync(Tournament tournament)
    {
        _context.Tournaments.Update(tournament);
        await _context.SaveChangesAsync();
    }

    public async Task DeleteAsync(Guid id)
    {
        var tournament = await _context.Tournaments.FindAsync(id);
        if (tournament != null)
        {
            _context.Tournaments.Remove(tournament);
            await _context.SaveChangesAsync();
        }
    }
}
```

---

### **Step 4: API Project**

#### **Dependency Injection**

Configure services in `Tournament.Api`.

`Tournament.Api/Program.cs`:

```csharp
using Microsoft.EntityFrameworkCore;
using Tournament.Core.Interfaces;
using Tournament.Data;
using Tournament.Data.Repositories;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
builder.Services.AddDbContext<TournamentDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

builder.Services.AddScoped<ITournamentRepository, TournamentRepository>();

builder.Services.AddControllers();

var app = builder.Build();

// Configure the HTTP request pipeline.
app.UseHttpsRedirection();
app.UseAuthorization();

app.MapControllers();

app.Run();
```

#### **Tournament Controller**

`Tournament.Api/Controllers/TournamentController.cs`:

```csharp
using Microsoft.AspNetCore.Mvc;
using Tournament.Core.Entities;
using Tournament.Core.Interfaces;

namespace Tournament.Api.Controllers;

[ApiController]
[Route("api/[controller]")]
public class TournamentController : ControllerBase
{
    private readonly ITournamentRepository _repository;

    public TournamentController(ITournamentRepository repository)
    {
        _repository = repository;
    }

    [HttpGet]
    public async Task<IActionResult> GetAllTournaments()
    {
        var tournaments = await _repository.GetAllAsync();
        return Ok(tournaments);
    }

    [HttpGet("{id}")]
    public async Task<IActionResult> GetTournamentById(Guid id)
    {
        var tournament = await _repository.GetByIdAsync(id);
        return tournament == null ? NotFound() : Ok(tournament);
    }

    [HttpPost]
    public async Task<IActionResult> CreateTournament([FromBody] Tournament tournament)
    {
        await _repository.AddAsync(tournament);
        return CreatedAtAction(nameof(GetTournamentById), new { id = tournament.Id }, tournament);
    }

    [HttpDelete("{id}")]
    public async Task<IActionResult> DeleteTournament(Guid id)
    {
        await _repository.DeleteAsync(id);
        return NoContent();
    }
}
```

---

### **Step 5: Configuration and Database Migration**

1. Add connection string to `appsettings.json`:
    
    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=localhost;Database=TournamentDb;Trusted_Connection=True;"
      }
    }
    ```
    
2. Install Entity Framework Core tools:
    
    ```bash
    dotnet add Tournament.Data package Microsoft.EntityFrameworkCore.Design
    ```
    
3. Add a migration:
    
    ```bash
    dotnet ef migrations add InitialCreate --project Tournament.Data
    dotnet ef database update --project Tournament.Data
    ```
    

---

This structure and implementation will give you a clean, modular, and testable **Tournament** microservice. Let me know if you need further assistance!