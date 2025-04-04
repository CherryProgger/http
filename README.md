using Qbit.Models;

namespace Qbit.Services
{
    public class KPIAssignmentService
    {
        private readonly QbitDbContext _dbContext;
        public KPIAssignmentService(QbitDbContext context)
        {
            _dbContext = context;
        }

        public async Task<List<KPIAssignment>> GetKPIAssignments(string global_area, string run)
        {
            var result = await _dbContext.GetKPIAssignments(global_area, run, null);
            return result;
        }
        public async Task<List<KPIAssignment>> GetKPIAssignmentsByPerson(string global_area, string run, string person)
        {

            var result = await _dbContext.GetKPIAssignments(global_area, run, person);
            return result;
        }
        public async Task<List<RunStatus>> GetRunStatusForPeriod(string run)
        {

            var result = await _dbContext.GetRunStatus(run);
            return result;
        }

        public async Task<bool> UpdateKPIAssignment(KPIFieldValueRequest request)
        {
            var model = MapKPIFieldValue(request);
            var result = await _dbContext.UpdateKPIAssignment(model);
            if (!result)
            {
                return false;
            }
            else 
            {
                return true;
            }
        }
        private KPIFieldValueDb MapKPIFieldValue(KPIFieldValueRequest request) 
        {
            var kpiFieldValue = new KPIFieldValueDb()
            {
                Run = request.Run,
                Person = request.Person_Id,
                Item_Id = request.KPI_Id,
                KPI_Field = request.Field,
                KPI_Value = request.Value
            };
            return kpiFieldValue;
        }
    }
};



