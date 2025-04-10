@startuml
actor Client

==Persons service==
== Список людей с отсутствующими причинами увольнения ==
Client -> PersonsController : GET /api/Persons/MissingLeaveReasons
activate PersonsController

PersonsController -> PersonsLeaveReasonService : GetPersonsMissingLeaveReasonsAsync()
activate PersonsLeaveReasonService

PersonsLeaveReasonService -> AppDbContext : GetPersonsMissingLeaveReasonsAsync()
activate AppDbContext

AppDbContext -> SQL_Server : EXEC sp_personsLeaveReason
activate SQL_Server
SQL_Server --> AppDbContext : return set
deactivate SQL_Server

AppDbContext --> PersonsLeaveReasonService : List<LeaveReasonInfo>
deactivate AppDbContext

PersonsLeaveReasonService --> PersonsController : List<LeaveReasonInfo>
deactivate PersonsLeaveReasonService

PersonsController --> Client : 200 OK (JSON) 
PersonsController --> Client : 400 BadRequest
deactivate PersonsController

== Обновление причин увольнения ==
Client -> PersonsController : PUT /api/Persons/MissingLeaveReasons\nBody: [{personId, idLeaveReason}]
activate PersonsController

PersonsController -> PersonsLeaveReasonService : SetLeaveReasonsAsync(request)
activate PersonsLeaveReasonService

activate AppDbContext
loop
PersonsLeaveReasonService -> AppDbContext : UpdateLeaveReasonAsync(personId, idLeaveReason)

AppDbContext -> SQL_Server : EXEC sp_testLeaveReasonsProcedure @PersonId, @IdLeaveReason
activate SQL_Server
SQL_Server --> AppDbContext : int rowsAffected
deactivate SQL_Server

AppDbContext --> PersonsLeaveReasonService : return success/failure (bool)
end loop

deactivate AppDbContext

PersonsLeaveReasonService --> PersonsController : return result
deactivate PersonsLeaveReasonService

PersonsController --> Client : 200 OK 
PersonsController --> Client : 400 BadRequest
deactivate PersonsController

@enduml
