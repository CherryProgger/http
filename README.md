PS C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App> dotnet run
Using launch settings from C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App\Properties\launchSettings.json...
Building...
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: http://localhost:5175
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\Users\tmp_AKorshunov\Documents\Projects\Leave Reasons App
warn: Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionMiddleware[3]
      Failed to determine the https port for redirect.
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (1,333ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      exec [dbo].[sp_personsLeaveReason]
Stack overflow.
Repeated 13693 times:
--------------------------------
   at LeaveReasonSystem.Models.LeaveReasonInfo.get_Actual_Termination_Date_Raw()
--------------------------------
   at System.Text.Json.Serialization.Metadata.JsonPropertyInfo`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].GetMemberAndWriteJson(System.Object, System.Te
xt.Json.WriteStack ByRef, System.Text.Json.Utf8JsonWriter)
   at System.Text.Json.Serialization.Converters.ObjectDefaultConverter`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].OnTryWrite(System.Text.Json.Utf8JsonWr
iter, System.__Canon, System.Text.Json.JsonSerializerOptions, System.Text.Json.WriteStack ByRef)
   at System.Text.Json.Serialization.JsonConverter`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].TryWrite(System.Text.Json.Utf8JsonWriter, System.__Canon B
yRef, System.Text.Json.JsonSerializerOptions, System.Text.Json.WriteStack ByRef)
   at System.Text.Json.Serialization.Converters.ListOfTConverter`2[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.__Canon, System.Private.CoreLib, Vers
ion=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].OnWriteResume(System.Text.Json.Utf8JsonWriter, System.__Canon, System.Text.Json.JsonSerializerOptions, System.Text.Json.WriteStack ByRef)
   at System.Text.Json.Serialization.JsonCollectionConverter`2[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.__Canon, System.Private.CoreLib, Version=
9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].OnTryWrite(System.Text.Json.Utf8JsonWriter, System.__Canon, System.Text.Json.JsonSerializerOptions, System.Text.Json.WriteStack ByRef)
   at System.Text.Json.Serialization.JsonConverter`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].TryWrite(System.Text.Json.Utf8JsonWriter, System.__Canon B
yRef, System.Text.Json.JsonSerializerOptions, System.Text.Json.WriteStack ByRef)
   at System.Text.Json.Serialization.JsonConverter`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].WriteCore(System.Text.Json.Utf8JsonWriter, System.__Canon
ByRef, System.Text.Json.JsonSerializerOptions, System.Text.Json.WriteStack ByRef)
   at System.Text.Json.Serialization.Metadata.JsonTypeInfo`1+<SerializeAsync>d__12[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].MoveNext()
   at System.Runtime.CompilerServices.AsyncMethodBuilderCore.Start[[System.Text.Json.Serialization.Metadata.JsonTypeInfo`1+<SerializeAsync>d__12[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral,
PublicKeyToken=7cec85d7bea7798e]], System.Text.Json, Version=9.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51]](<SerializeAsync>d__12<System.__Canon> ByRef)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder.Start[[System.Text.Json.Serialization.Metadata.JsonTypeInfo`1+<SerializeAsync>d__12[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral,
PublicKeyToken=7cec85d7bea7798e]], System.Text.Json, Version=9.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51]](<SerializeAsync>d__12<System.__Canon> ByRef)
   at System.Text.Json.Serialization.Metadata.JsonTypeInfo`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].SerializeAsync(System.IO.Pipelines.PipeWriter, Sys
tem.__Canon, Int32, System.Threading.CancellationToken, System.Object)
   at System.Text.Json.Serialization.Metadata.JsonTypeInfo`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].SerializeAsync(System.IO.Pipelines.PipeWriter, Sys
tem.__Canon, System.Threading.CancellationToken, System.Object)
   at System.Text.Json.Serialization.Metadata.JsonTypeInfo`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].SerializeAsObjectAsync(System.IO.Pipelines.PipeWri
ter, System.Object, System.Threading.CancellationToken)
   at Microsoft.AspNetCore.Mvc.Formatters.SystemTextJsonOutputFormatter+<WriteResponseBodyAsync>d__5.MoveNext()
   at System.Runtime.CompilerServices.AsyncMethodBuilderCore.Start[[Microsoft.AspNetCore.Mvc.Formatters.SystemTextJsonOutputFormatter+<WriteResponseBodyAsync>d__5, Microsoft.AspNetCore.Mvc.Core, Version=9.0.0.0, Culture=
neutral, PublicKeyToken=adb9793829ddae60]](<WriteResponseBodyAsync>d__5 ByRef)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder.Start[[Microsoft.AspNetCore.Mvc.Formatters.SystemTextJsonOutputFormatter+<WriteResponseBodyAsync>d__5, Microsoft.AspNetCore.Mvc.Core, Version=9.0.0.0, Culture=
neutral, PublicKeyToken=adb9793829ddae60]](<WriteResponseBodyAsync>d__5 ByRef)
   at Microsoft.AspNetCore.Mvc.Formatters.SystemTextJsonOutputFormatter.WriteResponseBodyAsync(Microsoft.AspNetCore.Mvc.Formatters.OutputFormatterWriteContext, System.Text.Encoding)
   at Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter.WriteAsync(Microsoft.AspNetCore.Mvc.Formatters.OutputFormatterWriteContext)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ObjectResultExecutor.ExecuteAsyncCore(Microsoft.AspNetCore.Mvc.ActionContext, Microsoft.AspNetCore.Mvc.ObjectResult, System.Type, System.Object)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ObjectResultExecutor.ExecuteAsync(Microsoft.AspNetCore.Mvc.ActionContext, Microsoft.AspNetCore.Mvc.ObjectResult)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker.ResultNext[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.__Canon, System.Private.CoreLib
, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]](State ByRef, Scope ByRef, System.Object ByRef, Boolean ByRef)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker.InvokeNextResultFilterAsync[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.__Canon, Syste
m.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]]()
   at Microsoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker.ResultNext[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.__Canon, System.Private.CoreLib
, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]](State ByRef, Scope ByRef, System.Object ByRef, Boolean ByRef)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker.InvokeResultFilters()
   at Microsoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker.Next(State ByRef, Scope ByRef, System.Object ByRef, Boolean ByRef)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker+<<InvokeFilterPipelineAsync>g__Awaited|20_0>d.MoveNext()
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Mi
crosoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker+<<InvokeFilterPipelineAsync>g__Awaited|20_0>d, Microsoft.AspNetCore.Mvc.Core, Version=9.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].ExecutionContextCall
back(System.Object)
   at System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Mi
crosoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker+<<InvokeFilterPipelineAsync>g__Awaited|20_0>d, Microsoft.AspNetCore.Mvc.Core, Version=9.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext(System.Thre
ading.Thread)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Mi
crosoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker+<<InvokeFilterPipelineAsync>g__Awaited|20_0>d, Microsoft.AspNetCore.Mvc.Core, Version=9.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext()
   at System.Threading.Tasks.AwaitTaskContinuation.RunOrScheduleAction(System.Runtime.CompilerServices.IAsyncStateMachineBox, Boolean)
   at System.Threading.Tasks.Task.RunContinuations(System.Object)
   at System.Threading.Tasks.Task`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].TrySetResult(System.Threading.Tasks.VoidTaskResult)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].SetExistingTaskResult(Sys
tem.Threading.Tasks.Task`1<System.Threading.Tasks.VoidTaskResult>, System.Threading.Tasks.VoidTaskResult)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder.SetResult()
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker+<<InvokeInnerFilterAsync>g__Awaited|13_0>d.MoveNext()
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Mi
crosoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker+<<InvokeInnerFilterAsync>g__Awaited|13_0>d, Microsoft.AspNetCore.Mvc.Core, Version=9.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].ExecutionContex
tCallback(System.Object)
   at System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Mi
crosoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker+<<InvokeInnerFilterAsync>g__Awaited|13_0>d, Microsoft.AspNetCore.Mvc.Core, Version=9.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext(System
.Threading.Thread)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Mi
crosoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker+<<InvokeInnerFilterAsync>g__Awaited|13_0>d, Microsoft.AspNetCore.Mvc.Core, Version=9.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext()
   at System.Threading.Tasks.AwaitTaskContinuation.RunOrScheduleAction(System.Runtime.CompilerServices.IAsyncStateMachineBox, Boolean)
   at System.Threading.Tasks.Task.RunContinuations(System.Object)
   at System.Threading.Tasks.Task`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].TrySetResult(System.Threading.Tasks.VoidTaskResult)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].SetExistingTaskResult(Sys
tem.Threading.Tasks.Task`1<System.Threading.Tasks.VoidTaskResult>, System.Threading.Tasks.VoidTaskResult)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder.SetResult()
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker+<<InvokeNextActionFilterAsync>g__Awaited|10_0>d.MoveNext()
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Mi
crosoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker+<<InvokeNextActionFilterAsync>g__Awaited|10_0>d, Microsoft.AspNetCore.Mvc.Core, Version=9.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].ExecutionC
ontextCallback(System.Object)
   at System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Mi
crosoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker+<<InvokeNextActionFilterAsync>g__Awaited|10_0>d, Microsoft.AspNetCore.Mvc.Core, Version=9.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext(S
ystem.Threading.Thread)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Mi
crosoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker+<<InvokeNextActionFilterAsync>g__Awaited|10_0>d, Microsoft.AspNetCore.Mvc.Core, Version=9.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext()

   at System.Threading.Tasks.AwaitTaskContinuation.RunOrScheduleAction(System.Runtime.CompilerServices.IAsyncStateMachineBox, Boolean)
   at System.Threading.Tasks.Task.RunContinuations(System.Object)
   at System.Threading.Tasks.Task`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].TrySetResult(System.Threading.Tasks.VoidTaskResult)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].SetExistingTaskResult(Sys
tem.Threading.Tasks.Task`1<System.Threading.Tasks.VoidTaskResult>, System.Threading.Tasks.VoidTaskResult)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder.SetResult()
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker+<<InvokeNextActionFilterAsync>g__Awaited|10_0>d.MoveNext()
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Mi
crosoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker+<<InvokeNextActionFilterAsync>g__Awaited|10_0>d, Microsoft.AspNetCore.Mvc.Core, Version=9.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].ExecutionC
ontextCallback(System.Object)
   at System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Mi
crosoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker+<<InvokeNextActionFilterAsync>g__Awaited|10_0>d, Microsoft.AspNetCore.Mvc.Core, Version=9.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext(S
ystem.Threading.Thread)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Mi
crosoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker+<<InvokeNextActionFilterAsync>g__Awaited|10_0>d, Microsoft.AspNetCore.Mvc.Core, Version=9.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext()

   at System.Threading.Tasks.AwaitTaskContinuation.RunOrScheduleAction(System.Runtime.CompilerServices.IAsyncStateMachineBox, Boolean)
   at System.Threading.Tasks.Task.RunContinuations(System.Object)
   at System.Threading.Tasks.Task`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].TrySetResult(System.Threading.Tasks.VoidTaskResult)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].SetExistingTaskResult(Sys
tem.Threading.Tasks.Task`1<System.Threading.Tasks.VoidTaskResult>, System.Threading.Tasks.VoidTaskResult)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder.SetResult()
   at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker+<<InvokeActionMethodAsync>g__Awaited|12_0>d.MoveNext()
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Mi
crosoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker+<<InvokeActionMethodAsync>g__Awaited|12_0>d, Microsoft.AspNetCore.Mvc.Core, Version=9.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].ExecutionConte
xtCallback(System.Object)
   at System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Mi
crosoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker+<<InvokeActionMethodAsync>g__Awaited|12_0>d, Microsoft.AspNetCore.Mvc.Core, Version=9.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext(Syste
m.Threading.Thread)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Threading.Tasks.VoidTaskResult, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Mi
crosoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker+<<InvokeActionMethodAsync>g__Awaited|12_0>d, Microsoft.AspNetCore.Mvc.Core, Version=9.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext()
   at System.Threading.Tasks.AwaitTaskContinuation.RunOrScheduleAction(System.Runtime.CompilerServices.IAsyncStateMachineBox, Boolean)
   at System.Threading.Tasks.Task.RunContinuations(System.Object)
   at System.Threading.Tasks.Task`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].TrySetResult(System.__Canon)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].SetExistingTaskResult(System.Threading.Tasks.Tas
k`1<System.__Canon>, System.__Canon)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ActionMethodExecutor+AwaitableObjectResultExecutor+<Execute>d__0.MoveNext()
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Microsoft.AspNetCore.Mvc.
Infrastructure.ActionMethodExecutor+AwaitableObjectResultExecutor+<Execute>d__0, Microsoft.AspNetCore.Mvc.Core, Version=9.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].ExecutionContextCallback(System.Object)
   at System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Microsoft.AspNetCore.Mvc.
Infrastructure.ActionMethodExecutor+AwaitableObjectResultExecutor+<Execute>d__0, Microsoft.AspNetCore.Mvc.Core, Version=9.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext(System.Threading.Thread)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Microsoft.AspNetCore.Mvc.
Infrastructure.ActionMethodExecutor+AwaitableObjectResultExecutor+<Execute>d__0, Microsoft.AspNetCore.Mvc.Core, Version=9.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext()
   at System.Threading.Tasks.AwaitTaskContinuation.RunOrScheduleAction(System.Action, Boolean)
   at System.Threading.Tasks.Task.RunContinuations(System.Object)
   at System.Threading.Tasks.Task`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].TrySetResult(System.__Canon)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].SetExistingTaskResult(System.Threading.Tasks.Tas
k`1<System.__Canon>, System.__Canon)
   at MissingLeaveReason.Controllers.MissingLeaveReasonController+<GetLeaveReasonAsync>d__3.MoveNext()
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.__Canon, System.Pr
ivate.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].ExecutionContextCallback(System.Object)
   at System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.__Canon, System.Pr
ivate.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].MoveNext(System.Threading.Thread)
   at System.Threading.Tasks.AwaitTaskContinuation.RunOrScheduleAction(System.Runtime.CompilerServices.IAsyncStateMachineBox, Boolean)
   at System.Threading.Tasks.Task.RunContinuations(System.Object)
   at System.Threading.Tasks.Task`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].TrySetResult(System.__Canon)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].SetExistingTaskResult(System.Threading.Tasks.Tas
k`1<System.__Canon>, System.__Canon)
   at LeaveReasonSystem.Services.LeaveReasonService+<GetAllPersonsAsync>d__6.MoveNext()
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.__Canon, System.Pr
ivate.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].ExecutionContextCallback(System.Object)
   at System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.__Canon, System.Pr
ivate.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].MoveNext(System.Threading.Thread)
   at System.Threading.Tasks.AwaitTaskContinuation.RunOrScheduleAction(System.Runtime.CompilerServices.IAsyncStateMachineBox, Boolean)
   at System.Threading.Tasks.Task.RunContinuations(System.Object)
   at System.Threading.Tasks.Task`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].TrySetResult(System.__Canon)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].SetExistingTaskResult(System.Threading.Tasks.Tas
k`1<System.__Canon>, System.__Canon)
   at LeaveReasonSystem.Data.LeaveReasonDbContext+<GetLeaveReasonRecordFromProcedureAsync>d__6.MoveNext()
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.__Canon, System.Pr
ivate.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].ExecutionContextCallback(System.Object)
   at System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.__Canon, System.Pr
ivate.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].MoveNext(System.Threading.Thread)
   at System.Threading.Tasks.AwaitTaskContinuation.RunOrScheduleAction(System.Runtime.CompilerServices.IAsyncStateMachineBox, Boolean)
   at System.Threading.Tasks.Task.RunContinuations(System.Object)
   at System.Threading.Tasks.Task`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].TrySetResult(System.__Canon)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].SetExistingTaskResult(System.Threading.Tasks.Tas
k`1<System.__Canon>, System.__Canon)
   at Microsoft.EntityFrameworkCore.EntityFrameworkQueryableExtensions+<ToListAsync>d__67`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].MoveNext()
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Microsoft.EntityFramework
Core.EntityFrameworkQueryableExtensions+<ToListAsync>d__67`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]], Microsoft.EntityFrameworkCore, Version=9.0.3.0, C
ulture=neutral, PublicKeyToken=adb9793829ddae60]].ExecutionContextCallback(System.Object)
   at System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Microsoft.EntityFramework
Core.EntityFrameworkQueryableExtensions+<ToListAsync>d__67`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]], Microsoft.EntityFrameworkCore, Version=9.0.3.0, C
ulture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext(System.Threading.Thread)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Microsoft.EntityFramework
Core.EntityFrameworkQueryableExtensions+<ToListAsync>d__67`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]], Microsoft.EntityFrameworkCore, Version=9.0.3.0, C
ulture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext()
   at System.Threading.Tasks.AwaitTaskContinuation.RunOrScheduleAction(System.Runtime.CompilerServices.IAsyncStateMachineBox, Boolean)
   at System.Threading.Tasks.Task.RunContinuations(System.Object)
   at System.Threading.Tasks.Task`1[[System.Boolean, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].TrySetResult(Boolean)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1[[System.Boolean, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].SetExistingTaskResult(System.Threading.Tasks.Tas
k`1<Boolean>, Boolean)
   at Microsoft.EntityFrameworkCore.Query.Internal.FromSqlQueryingEnumerable`1+AsyncEnumerator+<MoveNextAsync>d__19[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea779
8e]].MoveNext()
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Boolean, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Microsoft.EntityFramework
Core.Query.Internal.FromSqlQueryingEnumerable`1+AsyncEnumerator+<MoveNextAsync>d__19[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]], Microsoft.EntityFramework
Core.Relational, Version=9.0.3.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].ExecutionContextCallback(System.Object)
   at System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Boolean, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Microsoft.EntityFramework
Core.Query.Internal.FromSqlQueryingEnumerable`1+AsyncEnumerator+<MoveNextAsync>d__19[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]], Microsoft.EntityFramework
Core.Relational, Version=9.0.3.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext(System.Threading.Thread)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Boolean, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Microsoft.EntityFramework
Core.Query.Internal.FromSqlQueryingEnumerable`1+AsyncEnumerator+<MoveNextAsync>d__19[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]], Microsoft.EntityFramework
Core.Relational, Version=9.0.3.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext()
   at System.Threading.Tasks.AwaitTaskContinuation.RunOrScheduleAction(System.Runtime.CompilerServices.IAsyncStateMachineBox, Boolean)
   at System.Threading.Tasks.Task.RunContinuations(System.Object)
   at System.Threading.Tasks.Task`1[[System.Boolean, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].TrySetResult(Boolean)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1[[System.Boolean, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].SetExistingTaskResult(System.Threading.Tasks.Tas
k`1<Boolean>, Boolean)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1[[System.Boolean, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].SetResult(Boolean)
   at Microsoft.EntityFrameworkCore.SqlServer.Storage.Internal.SqlServerExecutionStrategy+<ExecuteAsync>d__7`2[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[
System.Boolean, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].MoveNext()
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Boolean, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Microsoft.EntityFramework
Core.SqlServer.Storage.Internal.SqlServerExecutionStrategy+<ExecuteAsync>d__7`2[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.Boolean, System.Private.
CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]], Microsoft.EntityFrameworkCore.SqlServer, Version=9.0.3.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].ExecutionContextCallback(System.O
bject)
   at System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Boolean, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Microsoft.EntityFramework
Core.SqlServer.Storage.Internal.SqlServerExecutionStrategy+<ExecuteAsync>d__7`2[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.Boolean, System.Private.
CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]], Microsoft.EntityFrameworkCore.SqlServer, Version=9.0.3.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext(System.Threading.Thread)

   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Boolean, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Microsoft.EntityFramework
Core.SqlServer.Storage.Internal.SqlServerExecutionStrategy+<ExecuteAsync>d__7`2[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.Boolean, System.Private.
CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]], Microsoft.EntityFrameworkCore.SqlServer, Version=9.0.3.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext()
   at System.Threading.Tasks.AwaitTaskContinuation.RunOrScheduleAction(System.Runtime.CompilerServices.IAsyncStateMachineBox, Boolean)
   at System.Threading.Tasks.Task.RunContinuations(System.Object)
   at System.Threading.Tasks.Task`1[[System.Boolean, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].TrySetResult(Boolean)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1[[System.Boolean, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].SetExistingTaskResult(System.Threading.Tasks.Tas
k`1<Boolean>, Boolean)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1[[System.Boolean, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].SetResult(Boolean)
   at Microsoft.EntityFrameworkCore.Query.Internal.FromSqlQueryingEnumerable`1+AsyncEnumerator+<InitializeReaderAsync>d__20[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85
d7bea7798e]].MoveNext()
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Boolean, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Microsoft.EntityFramework
Core.Query.Internal.FromSqlQueryingEnumerable`1+AsyncEnumerator+<InitializeReaderAsync>d__20[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]], Microsoft.EntityF
rameworkCore.Relational, Version=9.0.3.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].ExecutionContextCallback(System.Object)
   at System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Boolean, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Microsoft.EntityFramework
Core.Query.Internal.FromSqlQueryingEnumerable`1+AsyncEnumerator+<InitializeReaderAsync>d__20[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]], Microsoft.EntityF
rameworkCore.Relational, Version=9.0.3.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext(System.Threading.Thread)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.Boolean, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Microsoft.EntityFramework
Core.Query.Internal.FromSqlQueryingEnumerable`1+AsyncEnumerator+<InitializeReaderAsync>d__20[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]], Microsoft.EntityF
rameworkCore.Relational, Version=9.0.3.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext()
   at System.Threading.Tasks.AwaitTaskContinuation.RunOrScheduleAction(System.Runtime.CompilerServices.IAsyncStateMachineBox, Boolean)
   at System.Threading.Tasks.Task.RunContinuations(System.Object)
   at System.Threading.Tasks.Task`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].TrySetResult(System.__Canon)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].SetExistingTaskResult(System.Threading.Tasks.Tas
k`1<System.__Canon>, System.__Canon)
   at Microsoft.EntityFrameworkCore.Storage.RelationalCommand+<ExecuteReaderAsync>d__18.MoveNext()
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Microsoft.EntityFramework
Core.Storage.RelationalCommand+<ExecuteReaderAsync>d__18, Microsoft.EntityFrameworkCore.Relational, Version=9.0.3.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].ExecutionContextCallback(System.Object)
   at System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Microsoft.EntityFramework
Core.Storage.RelationalCommand+<ExecuteReaderAsync>d__18, Microsoft.EntityFrameworkCore.Relational, Version=9.0.3.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext(System.Threading.Thread)
   at System.Runtime.CompilerServices.AsyncTaskMethodBuilder`1+AsyncStateMachineBox`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[Microsoft.EntityFramework
Core.Storage.RelationalCommand+<ExecuteReaderAsync>d__18, Microsoft.EntityFrameworkCore.Relational, Version=9.0.3.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext()
   at System.Threading.Tasks.AwaitTaskContinuation.RunOrScheduleAction(System.Runtime.CompilerServices.IAsyncStateMachineBox, Boolean)
   at System.Threading.Tasks.Task.RunContinuations(System.Object)
   at System.Threading.Tasks.Task.FinishSlow(Boolean)
   at System.Threading.Tasks.Task.ExecuteWithThreadLocal(System.Threading.Tasks.Task ByRef, System.Threading.Thread)
   at System.Threading.Tasks.ThreadPoolTaskScheduler.TryExecuteTaskInline(System.Threading.Tasks.Task, Boolean)
   at System.Threading.Tasks.TaskScheduler.TryRunInline(System.Threading.Tasks.Task, Boolean)
   at System.Threading.Tasks.TaskContinuation.InlineIfPossibleOrElseQueue(System.Threading.Tasks.Task, Boolean)
   at System.Threading.Tasks.ContinueWithTaskContinuation.Run(System.Threading.Tasks.Task, Boolean)
   at System.Threading.Tasks.Task.RunContinuations(System.Object)
   at System.Threading.Tasks.Task`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].TrySetResult(System.__Canon)
   at System.Threading.Tasks.UnwrapPromise`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].TrySetFromTask(System.Threading.Tasks.Task, Boolean)
   at System.Threading.Tasks.UnwrapPromise`1[[System.__Canon, System.Private.CoreLib, Version=9.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].ProcessInnerTask(System.Threading.Tasks.Task)
   at System.Threading.Tasks.Task.RunContinuations(System.Object)
   at System.Threading.Tasks.Task.FinishSlow(Boolean)
   at System.Threading.Tasks.Task.ExecuteWithThreadLocal(System.Threading.Tasks.Task ByRef, System.Threading.Thread)
   at System.Threading.ThreadPoolWorkQueue.Dispatch()
   at System.Threading.PortableThreadPool+WorkerThread.WorkerThreadStart()
