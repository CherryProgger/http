using Microsoft.AspNetCore.Mvc;
using LeaveReasonsSystem.Services;
using LeaveReasonsSystem.Models;

namespace LeaveReasonsSystem.Controllers;

[ApiController]
[Route("/api/LeaveReasons")]
public class LeaveReasonsController  : ControllerBase
{
    private readonly LeaveReasonsService _LeaveReasonsService;
    private readonly ILogger<LeaveReasonsController > _logger;
    public LeaveReasonsController (LeaveReasonsService LeaveReasonsService, ILogger<LeaveReasonsController > logger)
    {
        _LeaveReasonsService = LeaveReasonsService;
        _logger = logger;
    }

    [HttpGet("", Name = "GetLeaveReasonsAsync")]
    public async Task<ActionResult<List<LeaveReasons>>> GetLeaveReasonsAsync()
    {
        try
        {
            var result = await _LeaveReasonsService.GetLeaveReasonsAsync();

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


using Microsoft.AspNetCore.Mvc;
using LeaveReasonsSystem.Services;
using LeaveReasonsSystem.Models;

namespace LeaveReasonsSystem.Controllers
{
    [ApiController]
    [Route("/api/Persons")]
    public class PersonsController : ControllerBase
    {
        private readonly PersonsLeaveReasonService _PersonsLeaveReasonService;
        private readonly ILogger<PersonsController> _logger;
        public PersonsController(PersonsLeaveReasonService PersonsLeaveReasonService, ILogger<PersonsController> logger)
        {
            _PersonsLeaveReasonService = PersonsLeaveReasonService;
            _logger = logger;
        }

        [HttpGet("MissingLeaveReasons", Name = "GetPersonsMissingLeaveReasonAsync")]
        public async Task<ActionResult<List<LeaveReasonInfo>>> GetPersonsMissingLeaveReasonAsync()
        {
            try
            {
                var result = await _PersonsLeaveReasonService.GetPersonsAsync();
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
        
        [HttpPut("MissingLeaveReasons", Name = "SetLeaveReasonAsync")]
        public async Task<IActionResult> SetLeaveReasonAsync([FromBody] List<LeaveReasonRecord> records)
        {
            var results = await _PersonsLeaveReasonService.SetLeaveReasonsAsync(records);
            var failed = results.Where(r => !r.success).Select(r => r.personId).ToList();

            if (failed.Count == 0)
                return Ok("All reasons updated");

            return StatusCode(207, new { message = "Some reasons failed", failedIds = failed });
        }

    }
}



using Microsoft.EntityFrameworkCore;
using LeaveReasonsSystem.Models;

namespace LeaveReasonsSystem.Data
{
    public class AppDbContext(DbContextOptions<AppDbContext> options) : DbContext(options)
    {
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<LeaveReasonInfo>().HasNoKey();
            modelBuilder.Entity<LeaveReasons>().HasNoKey();
            base.OnModelCreating(modelBuilder);
        }
        public async Task<List<LeaveReasonInfo>> GetPersonsMissingLeaveReasonAsync()
        {
            return await Set<LeaveReasonInfo>()
                .FromSqlRaw("exec [dbo].[sp_personsLeaveReason]")
                .ToListAsync();
        }

        public async Task<bool> UpdateLeaveReasonAsync(int PersonId, int IdLeaveReason)
        {
            var LR_PersonId = new Microsoft.Data.SqlClient.SqlParameter("@PersonId", PersonId);
            var LR_IdLeaveReason = new Microsoft.Data.SqlClient.SqlParameter("@IdLeaveReason", IdLeaveReason);

            var result = await Database.ExecuteSqlRawAsync(
                "exec [dbo].[sp_testLeaveReasonsProcedure] @PersonId={0}, @IdLeaveReason={1}",
                LR_PersonId, LR_IdLeaveReason);
            return result > 0;
        }

        public async Task<List<LeaveReasons>> GetLeaveReasonsDictionaryAsync()
        {
            return await Set<LeaveReasons>()
                .FromSqlRaw("exec [dbo].[sp_leaveReason]")
                .ToListAsync();
        }
    }
}



using Microsoft.EntityFrameworkCore;
using System.Text.Json.Serialization;
using System.Globalization;

namespace LeaveReasonsSystem.Models
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

namespace LeaveReasonsSystem.Models
{
    public class LeaveReasonRecord
    {
        [JsonPropertyName("personId")]
        public int PersonId { get; set; }
        [JsonPropertyName("idLeaveReason")]
        public int IdLeaveReason { get; set; }
    }
}


using Microsoft.EntityFrameworkCore;

namespace LeaveReasonsSystem.Models
{
    [Keyless]
    public record LeaveReasons
    {
        public int RowNumber { get; set; }
        public int ID { get; set; }
        public string Name { get; set; } = string.Empty;
        public string NameLocal { get; set; } = string.Empty;
        public string LeaveType { get; set; } = string.Empty;
        public string LeaveTypeLocal { get; set; } = string.Empty;
    }
}


{
  "$schema": "https://json.schemastore.org/launchsettings.json",
  "profiles": {
    "http": {
      "commandName": "Project",
      "dotnetRunMessages": true,
      "launchBrowser": false,
      "applicationUrl": "http://localhost:5175",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    },
    "https": {
      "commandName": "Project",
      "dotnetRunMessages": true,
      "launchBrowser": false,
      "applicationUrl": "https://localhost:7012;http://localhost:5175",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}


using LeaveReasonsSystem.Data;
using LeaveReasonsSystem.Models;

namespace LeaveReasonsSystem.Services
{
    public class LeaveReasonsService
    {
        private readonly AppDbContext _context;
        public LeaveReasonsService(AppDbContext context)
        {
            _context = context;
        }

        public async Task<List<LeaveReasons>> GetLeaveReasonsAsync()
        {
            return await _context.GetLeaveReasonsDictionaryAsync();
        }
    }

}


using LeaveReasonsSystem.Data;
using LeaveReasonsSystem.Models;

namespace LeaveReasonsSystem.Services
{
    public class PersonsLeaveReasonService
    {
        private readonly AppDbContext _context;
        private readonly ILogger<PersonsLeaveReasonService> _logger;

        public PersonsLeaveReasonService(AppDbContext context, ILogger<PersonsLeaveReasonService> logger)
        {
            _context = context;
            _logger = logger;
        }

        public async Task<List<LeaveReasonInfo>> GetPersonsAsync()
        {
            return await _context.GetPersonsMissingLeaveReasonAsync();
        }

        public async Task<List<(int personId, bool success)>> SetLeaveReasonsAsync(List<LeaveReasonRecord> requests)
        {
            var results = new List<(int personId, bool success)>();

            foreach (var request in requests)
            {
                try
                {
                    var success = await _context.UpdateLeaveReasonAsync(request.PersonId, request.IdLeaveReason);
                    results.Add((request.PersonId, success));

                    if (!success)
                        _logger.LogWarning($"Failed to update reason for termination for PersonId {request.PersonId}");
                }
                catch (Exception ex)
                {
                    _logger.LogError(ex, $"Failed to update PersonId {request.PersonId}");
                    results.Add((request.PersonId, false));
                }
            }
            return results;
        }
    }
}



using Microsoft.EntityFrameworkCore;
using Microsoft.OpenApi.Models;
using LeaveReasonsSystem.Data;
using LeaveReasonsSystem.Services;

var builder = WebApplication.CreateBuilder(args);

//1. DbContext
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

//2. Services
builder.Services.AddScoped<PersonsLeaveReasonService>();
builder.Services.AddScoped<LeaveReasonsService>();

//3. Controllers
builder.Services.AddControllers();

//4. Swagger
builder.Services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new OpenApiInfo { Title = "LeaveReasonService", Version = "v1" });
});

//5. CORS (важно: ДО builder.Build())
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowFrontend",
        policy => policy
            .WithOrigins("http://localhost:5173")
            .AllowAnyMethod()
            .AllowAnyHeader());
});

//6. Build
var app = builder.Build();

//7. Dev environment
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI(c =>
    {
        c.SwaggerEndpoint("/swagger/v1/swagger.json", "LeaveReasonService v1");
    });
}

//8. Middleware
app.UseCors("AllowFrontend");
app.UseHttpsRedirection();
app.UseAuthorization();
app.MapControllers();

app.Run();






