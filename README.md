public async Task<bool> UpdateLeaveReasonAsync(int personId, string leaveReason)
{
    var result = await Database.ExecuteSqlRawAsync(
        "EXEC sp_updateLeaveReason @p0, @p1",
        parameters: new object[] { personId, leaveReason });

    return result > 0; // вернет true, если хотя бы одна строка обновлена
}



public async Task<bool> PutLeaveReasonAsync(LeaveReasonRecord request)
{
    return await _dbContext.UpdateLeaveReasonAsync(request.PersonId, request.LeaveReason);
}


