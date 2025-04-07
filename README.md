PS C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App> dotnet build
Restore complete (0,8s)
  Leave Reasons App failed with 10 error(s) and 1 warning(s) (0,6s)
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\controllers\MissingLeaveReasonController.cs(34,24): error CS0029: Cannot implicitly convert type 'Microsoft.AspNetCore.Mvc.OkObjectResult' to 'System.Colle
ctions.Generic.List<LeaveReasonSystem.Models.LeaveReasonInfo>'
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\controllers\MissingLeaveReasonController.cs(37,20): error CS0029: Cannot implicitly convert type 'Microsoft.AspNetCore.Mvc.OkObjectResult' to 'System.Colle
ctions.Generic.List<LeaveReasonSystem.Models.LeaveReasonInfo>'
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\controllers\MissingLeaveReasonController.cs(42,31): error CS0103: The name 'result' does not exist in the current context
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\Data\leaveReasonDBContext.cs(20,14): error CS1061: 'DbSet<LeaveReasonInfo>' does not contain a definition for 'FromSqlRaw' and no accessible extension meth
od 'FromSqlRaw' accepting a first argument of type 'DbSet<LeaveReasonInfo>' could be found (are you missing a using directive or an assembly reference?)
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\services\LeaveReasonService.cs(22,17): error CS0117: 'LeaveReasonInfo' does not contain a definition for 'RowNumber'
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\services\LeaveReasonService.cs(28,20): warning CS8619: Nullability of reference types in value of type '?' doesn't match target type 'List<LeaveReasonInfo>
'.
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\Program.cs(10,10): error CS1061: 'IServiceCollection' does not contain a definition for 'AddSwaggerGen' and no accessible extension method 'AddSwaggerGen'
accepting a first argument of type 'IServiceCollection' could be found (are you missing a using directive or an assembly reference?)
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\Program.cs(16,23): error CS0246: The type or namespace name 'LeaveReasonDBContext' could not be found (are you missing a using directive or an assembly ref
erence?)
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\Program.cs(17,9): error CS1061: 'DbContextOptionsBuilder' does not contain a definition for 'UseNpgsql' and no accessible extension method 'UseNpgsql' acce
pting a first argument of type 'DbContextOptionsBuilder' could be found (are you missing a using directive or an assembly reference?)
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\Program.cs(23,9): error CS1061: 'WebApplication' does not contain a definition for 'UseSwagger' and no accessible extension method 'UseSwagger' accepting a
 first argument of type 'WebApplication' could be found (are you missing a using directive or an assembly reference?)
    C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\Program.cs(23,27): error CS1061: 'WebApplication' does not contain a definition for 'UseSwaggerUI' and no accessible extension method 'UseSwaggerUI' accept
ing a first argument of type 'WebApplication' could be found (are you missing a using directive or an assembly reference?)

Build failed with 10 error(s) and 1 warning(s) in 1,8s
