using Microsoft.AspNetCore.Builder;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.OpenApi.Models;
using LeaveReasonSystem.Data;
using LeaveReasonSystem.Services;
using AllLeaveReasonsSystem.Data;
using AllLeaveReasonsSystem.Services;

var builder = WebApplication.CreateBuilder(args);

// DbContexts
builder.Services.AddDbContext<LeaveReasonDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));
builder.Services.AddDbContext<AllLeaveReasonsDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

// Services
builder.Services.AddScoped<LeaveReasonService>();
builder.Services.AddScoped<AllLeaveReasonsService>();

builder.Services.AddControllers();

// Swagger configs
builder.Services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new OpenApiInfo { Title = "LeaveReasonService", Version = "v1" });
    c.SwaggerDoc("v2", new OpenApiInfo { Title = "AllLeaveReasonsService", Version = "v2" });
});

var app = builder.Build();

// Dev environment configuration
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI(c =>
    {
        // Обе вкладки в одном Swagger UI
        c.SwaggerEndpoint("/swagger/v1/swagger.json", "LeaveReasonService v1");
        c.SwaggerEndpoint("/swagger/v2/swagger.json", "AllLeaveReasonsService v2");
    });
}

// Common pipeline
app.UseHttpsRedirection();
app.UseRouting();
app.MapControllers();
app.Run();
