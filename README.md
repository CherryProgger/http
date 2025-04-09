@startuml LeaveReasonSystem

skinparam classAttributeIconSize 0

' ==== Models ====
class LeaveReasonInfo {
    +int RowNumber
    +int PersonId
    +int ActualTerminationDate
    +string ActualTerminationDateFormatted
    +string FullName
}

class LeaveReasonRecord {
    +int PersonId
    +string IdLeaveReason
}

' ==== DbContext ====
class LeaveReasonDbContext {
    +DbSet<LeaveReasonInfo> LeaveReasonResults
    +Task<List<LeaveReasonInfo>> GetLeaveReasonRecordFromProcedureAsync()
    +Task<bool> UpdateLeaveReasonAsync(int PersonId, string IdLeaveReason)
}

' ==== Service ====
class LeaveReasonService {
    -LeaveReasonDbContext _context
    +Task<List<LeaveReasonInfo>> GetAllPersonsAsync()
    +Task<bool> PutLeaveReasonAsync(LeaveReasonRecord request)
}

' ==== Controller ====
class MissingLeaveReasonController {
    -LeaveReasonService _LeaveReasonService
    -ILogger _logger
    +Task<ActionResult<List<LeaveReasonInfo>>> GetLeaveReasonAsync()
    +Task<IActionResult> PutLeaveReasonAsync(LeaveReasonRecord request)
}

' ==== Relations ====
LeaveReasonDbContext --> LeaveReasonInfo
LeaveReasonService --> LeaveReasonDbContext
MissingLeaveReasonController --> LeaveReasonService

@enduml
