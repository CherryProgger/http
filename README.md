[HttpGet("reasons", Name = "GetTerminationReasons")]
public async Task<ActionResult<List<TerminationInfo>>> GetTerminationReasons()
{
    try
    {
        var result = await _terminationService.GetAllTerminationReasons();

        if (result == null || result.Count == 0)
        {
            _logger.LogInformation("No termination records found.");
            return Ok(new List<TerminationInfo>());
        }

        return Ok(result);
    }
    catch (Exception ex)
    {
        _logger.LogError(ex, "Error while processing the GET request.");
        return BadRequest("An error occurred while fetching data.");
    }
}


using Microsoft.Extensions.Logging;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using TerminationSystem.Data;
using Microsoft.EntityFrameworkCore;

namespace TerminationSystem.Services
{
    // 🔹 DTO for response
    public class TerminationInfo
    {
        public int Id { get; set; }                    // Display index (not from DB)
        public string PersonId { get; set; } = null!;  // Unique system ID
        public string FullName { get; set; } = null!;  // Full name
        public DateTime TerminationDate { get; set; }  // Termination date
    }

    // 🔧 Service logic
    public class TerminationService
    {
        private readonly TerminationDbContext _context;
        private readonly ILogger<TerminationService> _logger;

        public TerminationService(TerminationDbContext context, ILogger<TerminationService> logger)
        {
            _context = context;
            _logger = logger;
        }

        public async Task<List<TerminationInfo>> GetAllTerminationReasons()
        {
            try
            {
                var records = await _context.Terminations
                    .OrderBy(t => t.TerminationDate)
                    .ToListAsync();

                var result = records.Select((record, index) => new TerminationInfo
                {
                    Id = index + 1,
                    PersonId = record.PersonId.ToString(),
                    FullName = record.FullName,
                    TerminationDate = record.TerminationDate
                }).ToList();

                return result;
            }
            catch (Exception ex)
            {
                _logger.LogError(ex, "Failed to load termination records.");
                return new List<TerminationInfo>();
            }
        }
    }
}



using Microsoft.EntityFrameworkCore;
using System;
using System.ComponentModel.DataAnnotations;

namespace TerminationSystem.Data
{
    public class TerminationRecord
    {
        [Key]
        public Guid PersonId { get; set; }
        public string FullName { get; set; } = null!;
        public DateTime TerminationDate { get; set; }
    }

    public class TerminationDbContext : DbContext
    {
        public TerminationDbContext(DbContextOptions<TerminationDbContext> options)
            : base(options)
        {
        }

        public DbSet<TerminationRecord> Terminations { get; set; } = null!;
    }
}


