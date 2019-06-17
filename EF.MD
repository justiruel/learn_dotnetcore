# Mysql EF

- fk
- unique
- index
- constraint

## Add package
```
dotnet add package Pomelo.EntityFrameworkCore.MySql --version 2.1.4
```

## appsettings.json
```
"ConnectionStrings": {
    "DefaultConnection": "server=localhost;port=3306;database=dbTodoApi;uid=root;password=root"
}
```

## Startup.cs
```
public void ConfigureServices(IServiceCollection services)
{

    services.AddDbContext<TodoContext>(options =>
    options.UseMySql(Configuration.GetConnectionString("DefaultConnection")));
    ....
}
```

## Guru.cs
```
namespace TodoApi.Models
{
    public class Guru
    {
        public int id { get; set; }
        public string nama { get; set; }
        public string jk { get; set; }
        public int umur { get; set; }
        //public int NIP { get; set; }
        
               
    }
}
```

## Siswa.cs
```
namespace TodoApi.Models
{
    public class Siswa
    {
        public int id { get; set; }
        public string nama { get; set; }
        public string jk { get; set; }
        public int umur { get; set; }
               
    }
}
```

## TodoContext.cs
```
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using TodoApi.Models;

namespace TodoApiDenganMysql.Models
{
    public class TodoContext : IdentityDbContext//DbContext
    {
        public TodoContext(DbContextOptions<TodoContext> options)
            : base(options)
        {
        }

        public DbSet<Siswa> siswa { get; set; } //nama harus sama dengan nama table
        public DbSet<Guru> guru   { get; set; }
    }
}
```

## Migration 
```
dotnet ef migrations add InitMigration -c TodoContext -o Migrations\TodoDB
dotnet ef database update
```

## ValuesController.cs
```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using TodoApi.Models;
using TodoApiDenganMysql.Models;

namespace Mysql.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class ValuesController : ControllerBase
    {
        private readonly TodoContext _context;
        public ValuesController(TodoContext context)
        {
            _context = context;
        }
        
        [HttpGet]
        public async Task<ActionResult<IEnumerable<Siswa>>> GetSiswa()
        {
            return await _context.siswa.ToListAsync();
        }

    }
}

```
