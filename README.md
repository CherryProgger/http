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
            // Указываем, что LeaveReasonInfo — keyless
            modelBuilder.Entity<LeaveReasonInfo>().HasNoKey();
            base.OnModelCreating(modelBuilder);
        }

        public async Task<List<LeaveReasonInfo>> GetLeaveReasonRecordFromProcedureAsync()
        {
            return await this.Set<LeaveReasonInfo>()
                .FromSqlRaw("exec [dbo].[sp_personsLeaveReasons]")
                .ToListAsync();
        }
    }
}
