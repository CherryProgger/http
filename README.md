@startuml
actor Client

== GET /api/LeaveReason/persons ==
Client -> MissingLeaveReasonController : GET /api/LeaveReason/persons
activate MissingLeaveReasonController

MissingLeaveReasonController -> LeaveReasonService : GetAllPersonsAsync()
activate LeaveReasonService

LeaveReasonService -> LeaveReasonDbContext : GetLeaveReasonFromProcedureAsync()
activate LeaveReasonDbContext

LeaveReasonDbContext -> SQL_Server : EXEC sp_personsLeaveReason
activate SQL_Server
SQL_Server --> LeaveReasonDbContext : List<LeaveReasonInfo>
deactivate SQL_Server

LeaveReasonDbContext --> LeaveReasonService : return persons list
deactivate LeaveReasonDbContext

LeaveReasonService --> MissingLeaveReasonController : return persons list
deactivate LeaveReasonService

MissingLeaveReasonController --> Client : 200 OK (JSON)
deactivate MissingLeaveReasonController

== PUT /api/LeaveReason/persons ==
Client -> MissingLeaveReasonController : PUT /api/LeaveReason/persons\nBody: {personId, idLeaveReason}
activate MissingLeaveReasonController

MissingLeaveReasonController -> LeaveReasonService : PutLeaveReasonAsync(request)
activate LeaveReasonService

LeaveReasonService -> LeaveReasonDbContext : UpdateLeaveReasonAsync(personId, idLeaveReason)
activate LeaveReasonDbContext

LeaveReasonDbContext -> SQL_Server : EXEC sp_testLeaveReasonsProcedure @PersonId, @IdLeaveReason
activate SQL_Server
SQL_Server --> LeaveReasonDbContext : int rowsAffected
deactivate SQL_Server

LeaveReasonDbContext --> LeaveReasonService : return success/failure (bool)
deactivate LeaveReasonDbContext

LeaveReasonService --> MissingLeaveReasonController : return result
deactivate LeaveReasonService

MissingLeaveReasonController --> Client : 200 OK / 400 BadRequest
deactivate MissingLeaveReasonController

==Leave Reason App==

@enduml
