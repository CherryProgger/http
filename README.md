public async Task<List<LeaveReasonInfo>> GetLeaveReasonRecordFromProcedureAsync()
{
    // Временно возвращаем заглушку
    return await Task.FromResult(new List<LeaveReasonInfo>
    {
        new LeaveReasonInfo
        {
            PersonId = 1,
            FullName = "Заглушка Иванов",
            LeaveReasonDate = DateTime.Now
        }
    });
}
