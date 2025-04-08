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

//DbContext
builder.Services.AddDbContext<LeaveReasonDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));
builder.Services.AddDbContext<AllLeaveReasonsDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

//Services
builder.Services.AddScoped<LeaveReasonService>();
builder.Services.AddScoped<AllLeaveReasonsService>();

builder.Services.AddControllers();

//Swaggers
builder.Services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new OpenApiInfo { Title = "LeaveReasonService", Version = "v1" });
});

builder.Services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v2", new OpenApiInfo { Title = "AllLeaveReasonsService", Version = "v2" });
});

var app = builder.Build();

//IsDev
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI(c =>
        c.SwaggerEndpoint("/swagger/v1/swagger.json", "LeaveReasonService v1"));
}
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI(c =>
        c.SwaggerEndpoint("/swagger/v2/swagger.json", "AllLeaveReasonsService v2"));
}

//AppsConnection
app.UseHttpsRedirection();
app.UseRouting();
app.MapControllers();
app.Run();
