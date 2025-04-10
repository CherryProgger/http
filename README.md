public class PersonsLeaveReasonService
{
    private readonly AppDbContext _context;
    private readonly ILogger<PersonsLeaveReasonService> _logger;

    public PersonsLeaveReasonService(AppDbContext context, ILogger<PersonsLeaveReasonService> logger)
    {
        _context = context;
        _logger = logger;
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
                    _logger.LogWarning($"Не удалось обновить причину увольнения для PersonId {request.PersonId}");
            }
            catch (Exception ex)
            {
                _logger.LogError(ex, $"Ошибка при обновлении PersonId {request.PersonId}");
                results.Add((request.PersonId, false));
            }
        }

        return results;
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
