using Microsoft.EntityFrameworkCore;
using LeaveReasonSystem.Models;
using AllLeaveReasonsSystem.Models;

namespace LeaveReasonSystem.Data
{
    public class AppDbContext : DbContext
    {
        public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) { }

        public DbSet<LeaveReasonInfo> LeaveReasonResults { get; set; } = null!;
        public DbSet<AllLeaveReasons> AllLeaveReasonsResults { get; set; } = null!;

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<LeaveReasonInfo>().HasNoKey();
            modelBuilder.Entity<AllLeaveReasons>().HasNoKey();
            base.OnModelCreating(modelBuilder);
        }

        // Процедуры
        public async Task<List<LeaveReasonInfo>> GetLeaveReasonFromProcedureAsync()
        {
            return await this.Set<LeaveReasonInfo>()
                .FromSqlRaw("exec [dbo].[sp_personsLeaveReason]")
                .ToListAsync();
        }

        public async Task<bool> UpdateLeaveReasonAsync(int PersonId, string IdLeaveReason)
        {
            var LR_PersonId = new Microsoft.Data.SqlClient.SqlParameter("@PersonId", PersonId);
            var LR_IdLeaveReason = new Microsoft.Data.SqlClient.SqlParameter("@IdLeaveReason", IdLeaveReason);

            var result = await Database.ExecuteSqlRawAsync(
                "exec [dbo].[sp_testLeaveReasonsProcedure] @PersonId={0}, @IdLeaveReason={1}",
                LR_PersonId, LR_IdLeaveReason);
            return result > 0;
        }

        public async Task<List<AllLeaveReasons>> GetAllLeaveReasonsFromProcedureAsync()
        {
            return await this.Set<AllLeaveReasons>()
                .FromSqlRaw("exec [dbo].[sp_leaveReason]")
                .ToListAsync();
        }
    }
}



// Было:
builder.Services.AddDbContext<LeaveReasonDbContext>(...);
builder.Services.AddDbContext<AllLeaveReasonsDbContext>(...);

// Стало:
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));



public class LeaveReasonService
{
    private readonly AppDbContext _context;
    public LeaveReasonService(AppDbContext context)
    {
        _context = context;
    }

    public async Task<List<LeaveReasonInfo>> GetAllPersonsAsync()
    {
        return await _context.GetLeaveReasonFromProcedureAsync();
    }

    public async Task<bool> PutLeaveReasonAsync(LeaveReasonRecord request)
    {
        return await _context.UpdateLeaveReasonAsync(request.PersonId, request.IdLeaveReason);
    }
}



public class AllLeaveReasonsService
{
    private readonly AppDbContext _context;
    public AllLeaveReasonsService(AppDbContext context)
    {
        _context = context;
    }

    public async Task<List<AllLeaveReasons>> GetAllLeaveReasonsAsync()
    {
        return await _context.GetAllLeaveReasonsFromProcedureAsync();
    }
}
