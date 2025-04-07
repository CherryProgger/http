fail: MissingLeaveReason.Controllers.MissingLeaveReasonController[0]
      Error while processing the GET request.
      System.InvalidOperationException: The ConnectionString property has not been initialized.
         at Npgsql.ThrowHelper.ThrowInvalidOperationException(String message)
         at Npgsql.NpgsqlConnection.Open(Boolean async, CancellationToken cancellationToken)
         at Npgsql.NpgsqlConnection.OpenAsync(CancellationToken cancellationToken)
         at Microsoft.EntityFrameworkCore.Storage.RelationalConnection.OpenDbConnectionAsync(Boolean errorsExpected, CancellationToken cancellationToken)
         at Microsoft.EntityFrameworkCore.Storage.RelationalConnection.OpenInternalAsync(Boolean errorsExpected, CancellationToken cancellationToken)
         at Microsoft.EntityFrameworkCore.Storage.RelationalConnection.OpenInternalAsync(Boolean errorsExpected, CancellationToken cancellationToken)
         at Microsoft.EntityFrameworkCore.Storage.RelationalConnection.OpenAsync(CancellationToken cancellationToken, Boolean errorsExpected)
         at Microsoft.EntityFrameworkCore.Storage.RelationalCommand.ExecuteReaderAsync(RelationalCommandParameterObject parameterObject, CancellationToken cancellationToken)
         at Microsoft.EntityFrameworkCore.Query.Internal.FromSqlQueryingEnumerable`1.AsyncEnumerator.InitializeReaderAsync(AsyncEnumerator enumerator, CancellationToken cancellationToken)
         at Npgsql.EntityFrameworkCore.PostgreSQL.Storage.Internal.NpgsqlExecutionStrategy.ExecuteAsync[TState,TResult](TState state, Func`4 operation, Func`4 verifySucceeded, CancellationToken cancellationToken)
         at Microsoft.EntityFrameworkCore.Query.Internal.FromSqlQueryingEnumerable`1.AsyncEnumerator.MoveNextAsync()
         at Microsoft.EntityFrameworkCore.EntityFrameworkQueryableExtensions.ToListAsync[TSource](IQueryable`1 source, CancellationToken cancellationToken)
         at Microsoft.EntityFrameworkCore.EntityFrameworkQueryableExtensions.ToListAsync[TSource](IQueryable`1 source, CancellationToken cancellationToken)
         at LeaveReasonSystem.Data.LeaveReasonDbContext.GetLeaveReasonRecordFromProcedureAsync() in C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\Data\leaveReasonDBContext.cs:line 19
         at LeaveReasonSystem.Services.LeaveReasonService.GetAllPersonsAsync() in C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\services\LeaveReasonService.cs:line 20
         at MissingLeaveReason.Controllers.MissingLeaveReasonController.GetLeaveReasonAsync() in C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\controllers\MissingLeaveReasonController.cs:line 29
