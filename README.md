
using Microsoft.EntityFrameworkCore;
using LeaveReasonSystem.Models;

namespace LeaveReasonSystem.Data
{
    public class LeaveReasonDbContext : DbContext
    {
        public LeaveReasonDbContext(DbContextOptions<LeaveReasonDbContext> options) : base(options) { }

        public DbSet<LeaveReasonInfo> LeaveReasonResults { get; set; } = null!;

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<LeaveReasonInfo>().HasNoKey();
            base.OnModelCreating(modelBuilder);
        }

        public async Task<List<LeaveReasonInfo>> GetLeaveReasonRecordFromProcedureAsync()
        {
            return await this.Set<LeaveReasonInfo>()
                .FromSqlRaw("exec [dbo].[sp_personsLeaveReason]")
                .ToListAsync();
        }

        public async Task<bool> UpdateLeaveReasonAsync(int PersonId, string IdLeaveReason)
        {
            var LR_PersonId = new Microsoft.Data.SqlClient.SqlParameter
            {
                ParameterName = "@PersonId",
                SqlDbType = System.Data.SqlDbType.Int,
                Size = -1,
                Value = PersonId,
            };

            var LR_IdLeaveReason = new Microsoft.Data.SqlClient.SqlParameter
            {
                ParameterName = "@IdLeaveReason",
                SqlDbType = System.Data.SqlDbType.NVarChar,
                Size = -1,
                Value = IdLeaveReason
            };

            var result = await Database.ExecuteSqlRawAsync(
                "exec [dbo].[sp_testLeaveReasonsProcedure] @PersonId={0}, @IdLeaveReason={1}", PersonId, IdLeaveReason);
            return result > 0;
        }
    }
}




fail: Microsoft.EntityFrameworkCore.Database.Command[20102]
      Failed executing DbCommand (7ms) [Parameters=[@p0='?' (DbType = Int32), @p1='?' (Size = 4000)], CommandType='Text', CommandTimeout='30']
      exec [dbo].[sp_testLeaveReasonsProcedure] @PersonId=@p0, @IdLeaveReason=@p1
fail: Microsoft.AspNetCore.Diagnostics.DeveloperExceptionPageMiddleware[1]
      An unhandled exception has occurred while executing the request.
      Microsoft.Data.SqlClient.SqlException (0x80131904): Procedure or function 'sp_testLeaveReasonsProcedure' expects parameter '@leaveReason', which was not supplied.
         at Microsoft.Data.SqlClient.SqlConnection.OnError(SqlException exception, Boolean breakConnection, Action`1 wrapCloseInAction)
         at Microsoft.Data.SqlClient.SqlInternalConnection.OnError(SqlException exception, Boolean breakConnection, Action`1 wrapCloseInAction)
         at Microsoft.Data.SqlClient.TdsParser.ThrowExceptionAndWarning(TdsParserStateObject stateObj, Boolean callerHasConnectionLock, Boolean asyncClose)
         at Microsoft.Data.SqlClient.TdsParser.TryRun(RunBehavior runBehavior, SqlCommand cmdHandler, SqlDataReader dataStream, BulkCopySimpleResultSet bulkCopyHandler, TdsParserStateObject stateObj, Boolean& dataReady)
         at Microsoft.Data.SqlClient.SqlCommand.FinishExecuteReader(SqlDataReader ds, RunBehavior runBehavior, String resetOptionsString, Boolean isInternal, Boolean forDescribeParameterEncryption, Boolean shouldCacheForAlwaysEncr
ypted)
         at Microsoft.Data.SqlClient.SqlCommand.CompleteAsyncExecuteReader(Boolean isInternal, Boolean forDescribeParameterEncryption)
         at Microsoft.Data.SqlClient.SqlCommand.InternalEndExecuteNonQuery(IAsyncResult asyncResult, Boolean isInternal, String endMethod)
         at Microsoft.Data.SqlClient.SqlCommand.EndExecuteNonQueryInternal(IAsyncResult asyncResult)
         at Microsoft.Data.SqlClient.SqlCommand.EndExecuteNonQueryAsync(IAsyncResult asyncResult)
         at Microsoft.Data.SqlClient.SqlCommand.<>c.<InternalExecuteNonQueryAsync>b__210_1(IAsyncResult result)
         at System.Threading.Tasks.TaskFactory`1.FromAsyncCoreLogic(IAsyncResult iar, Func`2 endFunction, Action`1 endAction, Task`1 promise, Boolean requiresSynchronization)
      --- End of stack trace from previous location ---
         at Microsoft.EntityFrameworkCore.Storage.RelationalCommand.ExecuteNonQueryAsync(RelationalCommandParameterObject parameterObject, CancellationToken cancellationToken)
         at Microsoft.EntityFrameworkCore.Storage.RelationalCommand.ExecuteNonQueryAsync(RelationalCommandParameterObject parameterObject, CancellationToken cancellationToken)
         at Microsoft.EntityFrameworkCore.Storage.RelationalCommand.ExecuteNonQueryAsync(RelationalCommandParameterObject parameterObject, CancellationToken cancellationToken)
         at Microsoft.EntityFrameworkCore.RelationalDatabaseFacadeExtensions.ExecuteSqlRawAsync(DatabaseFacade databaseFacade, String sql, IEnumerable`1 parameters, CancellationToken cancellationToken)
         at LeaveReasonSystem.Data.LeaveReasonDbContext.UpdateLeaveReasonAsync(Int32 PersonId, String IdLeaveReason) in C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\Data\leaveReasonDbContext.cs:line 43
         at LeaveReasonSystem.Services.LeaveReasonService.PutLeaveReasonAsync(LeaveReasonRecord request) in C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\services\LeaveReasonService.cs:line 24
         at MissingLeaveReason.Controllers.MissingLeaveReasonController.PutLeaveReasonAsync(LeaveReasonRecord request) in C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\controllers\MissingLeaveReasonController.cs:lin
e 48
         at Microsoft.AspNetCore.Mvc.Infrastructure.ActionMethodExecutor.TaskOfIActionResultExecutor.Execute(ActionContext actionContext, IActionResultTypeMapper mapper, ObjectMethodExecutor executor, Object controller, Object[] a
rguments)
         at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.<InvokeActionMethodAsync>g__Awaited|12_0(ControllerActionInvoker invoker, ValueTask`1 actionResultValueTask)
         at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.<InvokeNextActionFilterAsync>g__Awaited|10_0(ControllerActionInvoker invoker, Task lastTask, State next, Scope scope, Object state, Boolean isCompleted)
         at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.Rethrow(ActionExecutedContextSealed context)
         at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.Next(State& next, Scope& scope, Object& state, Boolean& isCompleted)
         at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.<InvokeInnerFilterAsync>g__Awaited|13_0(ControllerActionInvoker invoker, Task lastTask, State next, Scope scope, Object state, Boolean isCompleted)
         at Microsoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker.<InvokeFilterPipelineAsync>g__Awaited|20_0(ResourceInvoker invoker, Task lastTask, State next, Scope scope, Object state, Boolean isCompleted)
         at Microsoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker.<InvokeAsync>g__Awaited|17_0(ResourceInvoker invoker, Task task, IDisposable scope)
         at Microsoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker.<InvokeAsync>g__Awaited|17_0(ResourceInvoker invoker, Task task, IDisposable scope)
         at Swashbuckle.AspNetCore.SwaggerUI.SwaggerUIMiddleware.Invoke(HttpContext httpContext)
         at Swashbuckle.AspNetCore.Swagger.SwaggerMiddleware.Invoke(HttpContext httpContext, ISwaggerProvider swaggerProvider)
         at Microsoft.AspNetCore.Authorization.AuthorizationMiddleware.Invoke(HttpContext context)
         at Microsoft.AspNetCore.Authentication.AuthenticationMiddleware.Invoke(HttpContext context)
         at Microsoft.AspNetCore.Diagnostics.DeveloperExceptionPageMiddlewareImpl.Invoke(HttpContext context)
      ClientConnectionId:6716521d-ab79-4be8-99c4-177b33b01413
      Error Number:201,State:4,Class:16
