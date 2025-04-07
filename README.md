"ConnectionStrings": {
  "DefaultConnection": "Server=SERVER_NAME;Database=LeaveReasonDb;Trusted_Connection=True;"
}




using Microsoft.AspNetCore.Builder;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.OpenApi.Models;
using LeaveReasonSystem.Data;
using LeaveReasonSystem.Services;

var builder = WebApplication.CreateBuilder(args);

// Подключение к БД по Windows Authentication
builder.Services.AddDbContext<LeaveReasonDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

// Сервисы
builder.Services.AddScoped<LeaveReasonService>();

builder.Services.AddControllers();

// Swagger (для тестирования API)
builder.Services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new OpenApiInfo { Title = "LeaveReasonService", Version = "v1" });
});

var app = builder.Build();

// Swagger UI в режиме разработки
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI(c =>
        c.SwaggerEndpoint("/swagger/v1/swagger.json", "LeaveReasonService v1"));
}

app.UseHttpsRedirection();

app.UseRouting();

app.MapControllers();

app.Run();
