global using Microsoft.AspNetCore.Mvc;
global using LeaveReasonSystem.Services;


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
    
    [HttpGet("reasons", Name = "GetLeaveReasonAsync")]
    public async Task<List<LeaveReasonInfo>> GetLeaveReasonAsync()
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
            return BadRequest(result);
        }
    }
}
}



using Microsoft.EntityFrameworkCore; 
using System;
using System.ComponentModel.DataAnnotations;


public class LeaveReasonDbContext : DbContext
{
    public LeaveReasonDbContext(DbContextOptions<LeaveReasonDbContext> options) 
        : base(options)
    {
    }



    public async Task<List<LeaveReasonInfo>> GetLeaveReasonRecordFromProcedureAsync()
    {
        return await this.Set<LeaveReasonInfo>()
            .FromSqlRaw("exec [dbo].[sp_personsLeaveReasons]")
            .ToListAsync();
    }
}

namespace LeaveReasonSystem.Models
{


public class LeaveReasonInfo
{
    public int Id { get; set; }
    public int PersonId { get; set; } 
    public string FullName { get; set; } = null!;
    public DateTime LeaveReasonDate { get; set; }
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

        public async Task<List<LeaveReasonInfo>> GetAllPersonsAsync()
        {
            var records = await _context.GetLeaveReasonRecordFromProcedureAsync();

            var result = records.Select((record) => new LeaveReasonInfo
            {
                RowNumber = record.Id,
                PersonId = record.PersonId,
                FullName = record.FullName,
                LeaveReasonDate = record.LeaveReasonDate
            }).ToList();

            return result;
        }  
    }
}



using Microsoft.EntityFrameworkCore;
using Microsoft.OpenApi.Models;
using LeaveReasonSystem.Data;

var builder = WebApplication.CreateBuilder(args);
var services = builder.Services;

services.AddControllers();

services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new OpenApiInfo { Title = "LeaveReasonService", Version = "v1" });
});

services.AddScoped<LeaveReasonService>();

services.AddDbContext();

services.AddDbContext<LeaveReasonDbContext>(opt =>
    opt.UseNpgsql(builder.Configuration.GetConnectionString("DefaultConnection")));

var app = builder.Build();

if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI(c => c.SwaggerEndpoint("v1/swagger.json", "LeaveReasonService v1"));
}

app.UseHttpsRedirection();
app.UseRouting();
app.MapControllers();

app.Run();


ERRORS:
PS C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App> dotnet clean

Build succeeded in 0,8s
PS C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App> dotnet build
Restore complete (0,7s)
  Leave Reasons App failed with 4 error(s) (0,9s)
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\Program.cs(3,25): error CS0234: The type or namespace name 'Data' does not exist in the namespace 'LeaveReasonSystem' (are you missing an assembly referenc
e?)
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\services\LeaveReasonService.cs(2,25): error CS0234: The type or namespace name 'Data' does not exist in the namespace 'LeaveReasonSystem' (are you missing
an assembly reference?)
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\Data\leaveReasonDBContext.cs(16,28): error CS0246: The type or namespace name 'LeaveReasonInfo' could not be found (are you missing a using directive or an
 assembly reference?)
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\controllers\MissingLeaveReasonController.cs(23,28): error CS0246: The type or namespace name 'LeaveReasonInfo' could not be found (are you missing a using
directive or an assembly reference?)

Build failed with 4 error(s) in 2,0s
