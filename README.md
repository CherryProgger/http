namespace LeaveReasonSystem.Models
{
    public class LeaveReasonRecord
    {
        public int PersonId { get; set; }
        public string LeaveReason { get; set; } = string.Empty;
    }
}




public async Task<bool> UpdateLeaveReasonAsync(LeaveReasonRecord record)
{
    try
    {
        var sql = @"UPDATE [YourTableName]
                    SET [LeaveReasonColumn] = @reason
                    WHERE [PersonId] = @personId";

        var parameters = new[]
        {
            new SqlParameter("@reason", record.LeaveReason),
            new SqlParameter("@personId", record.PersonId)
        };

        await Database.ExecuteSqlRawAsync(sql, parameters);
        return true;
    }
    catch
    {
        return false;
    }
}




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

        public async Task<List<LeaveReasonInfo>> GetAllPersonsAsync()
        {
            return await _context.GetLeaveReasonRecordFromProcedureAsync();
        }

        public async Task<bool> PutLeaveReasonAsync(LeaveReasonRecord record)
        {
            return await _context.UpdateLeaveReasonAsync(record);
        }
    }
}




using LeaveReasonSystem.Data;
using LeaveReasonSystem.Services;
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

// Подключение DB
builder.Services.AddDbContext<LeaveReasonDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

// Сервисы
builder.Services.AddScoped<LeaveReasonService>();

// Контроллеры
builder.Services.AddControllers();

// Swagger
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

// Middleware
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();

app.UseAuthorization();

app.MapControllers();

app.Run();
