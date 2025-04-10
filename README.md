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
            bool allSucceeded = true;
            foreach (var record in records)
            {
                var result = await _PersonsLeaveReasonService.SetLeaveReasonAsync(record);
                if (!result)
                {
                    allSucceeded = false;
                    _logger.LogWarning($"Failed to update PersonId {record.PersonId}");
                }
            }
            return allSucceeded ? Ok("All reasons updated") : StatusCode(207, "Some reasons failed to update");
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
        public PersonsLeaveReasonService(AppDbContext context)
        {
            _context = context;
        }

        public async Task<List<LeaveReasonInfo>> GetPersonsAsync()
        {
            return await _context.GetPersonsMissingLeaveReasonAsync();
        }

        public async Task<bool> SetLeaveReasonAsync(LeaveReasonRecord request)
        {
            return await _context.UpdateLeaveReasonAsync(request.PersonId, request.IdLeaveReason);
        }
    }
}
