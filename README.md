using Microsoft.AspNetCore.Mvc;
using AllLeaveReasonsSystem.Services;
using AllLeaveReasonsSystem.Models;

namespace AllLeaveReasonsController.Controllers;

[ApiController]
[Route("/api/LeaveReason")]
public class AllLeaveReasonsController : ControllerBase
{
    private readonly AllLeaveReasonsService _AllLeaveReasonsService;
    private readonly ILogger<AllLeaveReasonsController> _logger;
    public AllLeaveReasonsController(AllLeaveReasonsService AllLeaveReasonsService, ILogger<AllLeaveReasonsController> logger)
    {
        _AllLeaveReasonsService = AllLeaveReasonsService;
        _logger = logger;
    }

    [HttpGet("allReasons", Name = "GetAllLeaveReasonsAsync")]
    public async Task<ActionResult<List<AllLeaveReasons>>> GetAllLeaveReasonsAsync()
    {
        try
        {
            var result = await _AllLeaveReasonsService.GetAllLeaveReasonsAsync();

            if (result.Count == 0)
            {
                _logger.LogInformation("No Leave Reasons records found.");
                return Ok(result);
            }
            return Ok(result);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Error while processing the GET request.");
            return BadRequest("ERROR");
        }
    }
}

global using Microsoft.AspNetCore.Mvc;
global using LeaveReasonSystem.Services;
using LeaveReasonSystem.Models;


namespace MissingLeaveReason.Controllers
{
    [ApiController]
    [Route("/api/LeaveReason")]
    public class MissingLeaveReasonController : ControllerBase
    {
        private readonly LeaveReasonService _LeaveReasonService;
        private readonly ILogger<MissingLeaveReasonController> _logger;
        public MissingLeaveReasonController(LeaveReasonService LeaveReasonService, ILogger<MissingLeaveReasonController> logger)
        {
            _LeaveReasonService = LeaveReasonService;
            _logger = logger;
        }
        
        [HttpGet("persons", Name = "GetLeaveReasonAsync")]
        public async Task<ActionResult<List<LeaveReasonInfo>>> GetLeaveReasonAsync()
        {
            try
            {
                var result = await _LeaveReasonService.GetAllPersonsAsync();

                if (result.Count == 0)
                {
                    _logger.LogInformation("No Leave Reason records found.");
                    return Ok(result);
                }

                return Ok(result);
            }
            catch (Exception ex)
            {
                _logger.LogError(ex, "Error while processing the GET request.");
                return BadRequest("ERROR");
            }
        }
        [HttpPut("persons", Name = "PutLeaveReasonAsync")]
        public async Task<IActionResult> PutLeaveReasonAsync([FromBody] LeaveReasonRecord request)
        {
            var result = await _LeaveReasonService.PutLeaveReasonAsync(request);
            if (!result)
            {
                return BadRequest(result);
            }
            return Ok(result);
        }
    }
}

using Microsoft.EntityFrameworkCore;
using LeaveReasonSystem.Models;
using AllLeaveReasonsSystem.Models;

namespace LeaveReasonSystem.Data
{
    public class AllLeaveReasonsDbContext : DbContext
    {
        public AllLeaveReasonsDbContext(DbContextOptions<AllLeaveReasonsDbContext> options) : base(options) { }

        public DbSet<AllLeaveReasons> AllLeaveReasonsResults { get; set; } = null!;

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<AllLeaveReasons>().HasNoKey();
            base.OnModelCreating(modelBuilder);
        }

        public async Task<List<AllLeaveReasons>> GetAllLeaveReasonsFromProcedureAsync()
        {
            return await this.Set<AllLeaveReasons>()
                .FromSqlRaw("exec [dbo].[sp_leaveReason]")
                .ToListAsync();
        }
    }
    public class LeaveReasonDbContext : DbContext
    {
        public LeaveReasonDbContext(DbContextOptions<LeaveReasonDbContext> options) : base(options) { }

        public DbSet<LeaveReasonInfo> LeaveReasonResults { get; set; } = null!;

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<LeaveReasonInfo>().HasNoKey();
            base.OnModelCreating(modelBuilder);
        }

        public async Task<List<LeaveReasonInfo>> GetLeaveReasonFromProcedureAsync()
        {
            return await this.Set<LeaveReasonInfo>()
                .FromSqlRaw("exec [dbo].[sp_personsLeaveReason]")
                .ToListAsync();
        }

        public async Task<bool> UpdateLeaveReasonAsync(int PersonId, string IdLeaveReason)
        {
            var LR_PersonId = new Microsoft.Data.SqlClient.SqlParameter
            {
                ParameterName = "@PersonId",
                SqlDbType = System.Data.SqlDbType.Int,
                Size = -1,
                Value = PersonId,
            };

            var LR_IdLeaveReason = new Microsoft.Data.SqlClient.SqlParameter
            {
                ParameterName = "@IdLeaveReason",
                SqlDbType = System.Data.SqlDbType.NVarChar,
                Size = -1,
                Value = IdLeaveReason
            };

            var result = await Database.ExecuteSqlRawAsync(
                "exec [dbo].[sp_testLeaveReasonsProcedure] @PersonId={0}, @IdLeaveReason={1}", LR_PersonId, LR_IdLeaveReason);
            return result > 0;
        }
    }
}



using Microsoft.EntityFrameworkCore;
using System.Text.Json.Serialization;
using System.Globalization;

namespace AllLeaveReasonsSystem.Models
{
    [Keyless]
    public record AllLeaveReasons
    {
        public int RowNumber { get; set; }
        public int ID { get; set; }
        public string Name { get; set; } = string.Empty;
        public string NameLocal { get; set; } = string.Empty;
        public string LeaveType { get; set; } = string.Empty;
        public string LeaveTypeLocal { get; set; } = string.Empty;
    }
}

using Microsoft.EntityFrameworkCore;
using System.Text.Json.Serialization;
using System.Globalization;

namespace LeaveReasonSystem.Models
{

    [Keyless]
    public record LeaveReasonInfo
    {
        public int RowNumber { get; set; }
        public int PersonId { get; set; }

        [JsonIgnore]
        public int ActualTerminationDate { get; set; }

        [JsonPropertyName("ActualTerminationDate")]
        public string ActualTerminationDateFormatted =>
            DateTime.ParseExact(
                ActualTerminationDate.ToString(),
                 "yyyyMMdd",
                 CultureInfo.InvariantCulture
            ).ToString("dd.MM.yyyy");
        public string FullName { get; set; } = string.Empty;

    }

}

using System.Text.Json.Serialization;

public class LeaveReasonRecord
{
    [JsonPropertyName("personId")]
    public int PersonId { get; set; }

    [JsonPropertyName("idLeaveReason")]
    public string IdLeaveReason { get; set; } = string.Empty;
}


using Microsoft.EntityFrameworkCore;
using LeaveReasonSystem.Data;
using AllLeaveReasonsSystem.Models;

namespace AllLeaveReasonsSystem.Services
{

    public class AllLeaveReasonsService
    {
        private readonly AllLeaveReasonsDbContext _context;

        public AllLeaveReasonsService(AllLeaveReasonsDbContext context)
        {
            _context = context;
        }

        public async Task<List<AllLeaveReasons>> GetAllLeaveReasonsAsync()
        {
            return await _context.GetAllLeaveReasonsFromProcedureAsync();
        }

    }

}

using Microsoft.EntityFrameworkCore;
using LeaveReasonSystem.Data;
using LeaveReasonSystem.Models;

namespace LeaveReasonSystem.Services
{
    public class LeaveReasonService
    {
        private readonly LeaveReasonDbContext _context;

        public LeaveReasonService(LeaveReasonDbContext context)
        {
            _context = context;
        }

        public DbSet<LeaveReasonInfo> LeaveReasons { get; set; } = null!;

        public async Task<List<LeaveReasonInfo>> GetAllPersonsAsync()
        {
            return await _context.GetLeaveReasonFromProcedureAsync();
        }
        public async Task<bool> PutLeaveReasonAsync(LeaveReasonRecord request)
        {
            return await _context.UpdateLeaveReasonAsync(request.PersonId, request.IdLeaveReason);
        }
    }
}


using Microsoft.AspNetCore.Builder;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.OpenApi.Models;
using LeaveReasonSystem.Data;
using LeaveReasonSystem.Services;
using AllLeaveReasonsSystem.Services;

var builder = WebApplication.CreateBuilder(args);

// DbContexts
builder.Services.AddDbContext<LeaveReasonDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));
builder.Services.AddDbContext<AllLeaveReasonsDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

// Services
builder.Services.AddScoped<LeaveReasonService>();
builder.Services.AddScoped<AllLeaveReasonsService>();

builder.Services.AddControllers();

// Swagger configs
builder.Services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new OpenApiInfo { Title = "LeaveReasonService", Version = "v1" });
    c.SwaggerDoc("v2", new OpenApiInfo { Title = "AllLeaveReasonsService", Version = "v2" });
});

var app = builder.Build();

// Dev environment configuration
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI(c =>
    {
        //One Swagger UI
        c.SwaggerEndpoint("/swagger/v1/swagger.json", "LeaveReasonService v1");
        c.SwaggerEndpoint("/swagger/v2/swagger.json", "AllLeaveReasonsService v2");
    });
}

// Common pipeline
app.UseHttpsRedirection();
app.UseRouting();
app.MapControllers();
app.Run();
