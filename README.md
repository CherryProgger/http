Мой Сервис


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
    public async Task<IActionResult>PutLeaveReasonAsync ([FromBody] LeaveReasonRecord request)
    {
    var result = await _LeaveReasonService.PutLeaveReasonAsync(request);
        if (!result)
        {
            return BadRequest(result);
        }
        else
            return Ok(result);
    }


}
}


Мой контроллер
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
    public async Task<IActionResult>PutLeaveReasonAsync ([FromBody] LeaveReasonRecord request)
    {
    var result = await _LeaveReasonService.PutLeaveReasonAsync(request);
        if (!result)
        {
            return BadRequest(result);
        }
        else
            return Ok(result);
    }


}
}



LeaveReasonInfo



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

    [JsonPropertyName("Actual_Termination_Date")]
    public string ActualTerminationDateFormatted =>
        DateTime.ParseExact(
            ActualTerminationDate.ToString(),
             "yyyyMMdd",
             CultureInfo.InvariantCulture
        ).ToString("dd.MM.yyyy");
    public string FullName { get; set; } = string.Empty;

}

}



Контекст

using Microsoft.EntityFrameworkCore;
using LeaveReasonSystem.Models;

namespace LeaveReasonSystem.Data 
{
    public class LeaveReasonDbContext : DbContext
    {
        public LeaveReasonDbContext(DbContextOptions<LeaveReasonDbContext> options) : base(options) {}

    public DbSet<LeaveReasonInfo> LeaveReasonResults { get; set; } = null!;

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<LeaveReasonInfo>().HasNoKey();
        base.OnModelCreating(modelBuilder);
    }

    public async Task<List<LeaveReasonInfo>> GetLeaveReasonRecordFromProcedureAsync()
    {
        return await this.Set<LeaveReasonInfo>()
            .FromSqlRaw("exec [dbo].[sp_personsLeaveReason]")
            .ToListAsync();
    }
}
}

