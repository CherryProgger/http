using Microsoft.EntityFrameworkCore;
using LeaveReasonSystem.Data;
using LeaveReasonSystem.Services;
using Microsoft.OpenApi.Models;

var builder = WebApplication.CreateBuilder(args);

// Подключение строки из конфигурации
var connectionString = builder.Configuration.GetConnectionString("DefaultConnection");

// Регистрация сервисов
builder.Services.AddControllers();
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new OpenApiInfo { Title = "LeaveReason API", Version = "v1" });
});

builder.Services.AddDbContext<LeaveReasonDbContext>(options =>
    options.UseNpgsql(connectionString));

builder.Services.AddScoped<LeaveReasonService>();

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







{
  "ConnectionStrings": {
    "DefaultConnection": "Host=localhost;Port=5432;Database=your_db_name;Username=your_user;Password=your_password"
  }
}
