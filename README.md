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

        public DbSet<LeaveReasonInfo> LeaveReasons { get; set; } = null!;

        public async Task<List<LeaveReasonInfo>> GetAllPersonsAsync()
        {
            var records = await _context.GetLeaveReasonRecordFromProcedureAsync();

            var result = records.Select((record) => new LeaveReasonInfo
            {
                RowNumber = record.RowNumber,
                Person_Id = record.Person_Id,
                Actual_Termination_Date = record.Actual_Termination_Date,
                Full_Name_Local = record.Full_Name_Local
            }).ToList();

            return result;
        }  
    }
}
