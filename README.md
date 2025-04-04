# http


using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;
using Microsoft.EntityFrameworkCore;

namespace YourNamespace.Services
{
    // DTO для вывода данных клиенту
    public class TerminationInfo
    {
        public int Id { get; set; } // Порядковый номер отображения
        public string PersonId { get; set; } = string.Empty; // Уникальный ID сотрудника
        public string FullName { get; set; } = string.Empty; // Полное имя
        public DateTime TerminationDate { get; set; } // Дата увольнения
    }

    // Entity-модель, отражающая таблицу в БД
    public class TerminationEntity
    {
        public Guid PersonId { get; set; } // Первичный ключ
        public string FullName { get; set; } = string.Empty;
        public DateTime TerminationDate { get; set; }
    }

    // Контекст базы данных
    public class ApplicationDbContext : DbContext
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options) { }

        public DbSet<TerminationEntity> TerminationRecords { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            base.OnModelCreating(modelBuilder);

            modelBuilder.Entity<TerminationEntity>(entity =>
            {
                entity.ToTable("TerminationRecords"); // 🔁 Название таблицы в SQL

                entity.HasKey(e => e.PersonId); // 🔑 Первичный ключ

                entity.Property(e => e.FullName)
                    .IsRequired()
                    .HasMaxLength(255);

                entity.Property(e => e.TerminationDate)
                    .IsRequired();
            });
        }
    }

    // Сервис для получения данных об увольнениях
    public class TerminationService
    {
        private readonly ApplicationDbContext _context;
        private readonly ILogger<TerminationService> _logger;

        public TerminationService(ApplicationDbContext context, ILogger<TerminationService> logger)
        {
            _context = context;
            _logger = logger;
        }

        // Получение всех записей увольнений
        public async Task<List<TerminationInfo>> GetAllTerminationsAsync()
        {
            try
            {
                var records = await _context.TerminationRecords
                    .OrderByDescending(x => x.TerminationDate)
                    .ToListAsync();

                var result = records.Select((record, index) => new TerminationInfo
                {
                    Id = index + 1,
                    PersonId = record.PersonId.ToString(),
                    FullName = record.FullName,
                    TerminationDate = record.TerminationDate
                }).ToList();

                return result;
            }
            catch (Exception ex)
            {
                _logger.LogError(ex, "Error while fetching termination records from database.");
                throw;
            }
        }
    }
}
