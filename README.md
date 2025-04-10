[HttpPut("persons/batch", Name = "PutLeaveReasonsBatchAsync")]
public async Task<IActionResult> PutLeaveReasonsBatchAsync([FromBody] List<LeaveReasonRecord> records)
{
    bool allSucceeded = true;

    foreach (var record in records)
    {
        var result = await _LeaveReasonService.PutLeaveReasonAsync(record);
        if (!result)
        {
            allSucceeded = false;
            _logger.LogWarning($"Не удалось обновить PersonId {record.PersonId}");
        }
    }

    return allSucceeded ? Ok("Все причины обновлены") : StatusCode(207, "Некоторые причины не удалось обновить");
}
