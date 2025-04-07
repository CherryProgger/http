Service:

using Microsoft.EntityFrameworkCore;
using LeaveReasonsSystem.Data;
using LeaveReasonsSystem.Models;

namespace LeaveReasonsSystem.Services
{
    public class LeaveReasonService
    {
        private readonly LeaveReasonsDbContext _context;

        private LeaveReasonService(LeaveReasonsDbContext context)
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


Controller:

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


DBContext:

using Microsoft.EntityFrameworkCore; 
using System;
using System.ComponentModel.DataAnnotations;


public class LeaveReasonDbContext : DbContext
{
    public LeaveReasonDbContext(DbContextOptions<LeaveReasonDbContext> options) 
        : base(options)
    {
    }

    //public DbSet<LeaveReasonInfo> leaveReason{get; set} = null!;


    public async Task<List<LeaveReasonInfo>> GetLeaveReasonRecordFromProcedureAsync()
    {
        return await this.Set<LeaveReasonInfo>()
            .FromQqlRaw("exec [dbo].[sp_personsLeaveReasons]")
            .ToListAsync();
    }
}
Programm.cs:

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

PS C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App> dotnet build
Restore complete (0,7s)
  Leave Reasons App failed with 11 error(s) (0,5s)
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\controllers\MissingLeaveReasonController.cs(2,32): error CS0234: The type or namespace name 'Services' does not exist in the namespace 'LeaveReasonSystem'
(are you missing an assembly reference?)
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\Program.cs(3,25): error CS0234: The type or namespace name 'Data' does not exist in the namespace 'LeaveReasonSystem' (are you missing an assembly referenc
e?)
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\services\LeaveReasonService.cs(2,26): error CS0234: The type or namespace name 'Data' does not exist in the namespace 'LeaveReasonsSystem' (are you missing
 an assembly reference?)
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\services\LeaveReasonService.cs(3,26): error CS0234: The type or namespace name 'Models' does not exist in the namespace 'LeaveReasonsSystem' (are you missi
ng an assembly reference?)
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\controllers\MissingLeaveReasonController.cs(23,28): error CS0246: The type or namespace name 'LeaveReasonInfo' could not be found (are you missing a using
directive or an assembly reference?)
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\services\LeaveReasonService.cs(16,32): error CS0246: The type or namespace name 'LeaveReasonInfo' could not be found (are you missing a using directive or
an assembly reference?)
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\Data\leaveReasonsDBContext.cs(16,28): error CS0246: The type or namespace name 'LeaveReasonInfo' could not be found (are you missing a using directive or a
n assembly reference?)
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\controllers\MissingLeaveReasonController.cs(13,22): error CS0246: The type or namespace name 'LeaveReasonService' could not be found (are you missing a usi
ng directive or an assembly reference?)
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\services\LeaveReasonService.cs(9,26): error CS0246: The type or namespace name 'LeaveReasonsDbContext' could not be found (are you missing a using directiv
e or an assembly reference?)
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\services\LeaveReasonService.cs(11,36): error CS0246: The type or namespace name 'LeaveReasonsDbContext' could not be found (are you missing a using directi
ve or an assembly reference?)
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\controllers\MissingLeaveReasonController.cs(16,41): error CS0246: The type or namespace name 'LeaveReasonService' could not be found (are you missing a usi
ng directive or an assembly reference?)

Build failed with 11 error(s) in 1,6s
