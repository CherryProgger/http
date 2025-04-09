@startuml

' Models
class LeaveReasonInfo {
    +int RowNumber
    +int PersonId
    +int ActualTerminationDate
    +string ActualTerminationDateFormatted
    +string FullName
}

class AllLeaveReasons {
    +int RowNumber
    +int ID
    +string Name
    +string NameLocal
    +string LeaveType
    +string LeaveTypeLocal
}

class LeaveReasonRecord {
    +int PersonId
    +string IdLeaveReason
}

' DbContext
class AppDbContext {
    +DbSet<LeaveReasonInfo> LeaveReasonResults
    +DbSet<AllLeaveReasons> AllLeaveReasonsResults
    +Task<List<LeaveReasonInfo>> GetLeaveReasonFromProcedureAsync()
    +Task<bool> UpdateLeaveReasonAsync(int, string)
    +Task<List<AllLeaveReasons>> GetAllLeaveReasonsFromProcedureAsync()
}

' Services
class LeaveReasonService {
    +Task<List<LeaveReasonInfo>> GetAllPersonsAsync()
    +Task<bool> PutLeaveReasonAsync(LeaveReasonRecord)
}

class AllLeaveReasonsService {
    +Task<List<AllLeaveReasons>> GetAllLeaveReasonsAsync()
}

' Controllers
class MissingLeaveReasonController {
    +Task<ActionResult<List<LeaveReasonInfo>>> GetLeaveReasonAsync()
    +Task<IActionResult> PutLeaveReasonAsync(LeaveReasonRecord)
}

class AllLeaveReasonsController {
    +Task<ActionResult<List<AllLeaveReasons>>> GetAllLeaveReasonsAsync()
}

' Relationships
LeaveReasonService --> AppDbContext
AllLeaveReasonsService --> AppDbContext

MissingLeaveReasonController --> LeaveReasonService
AllLeaveReasonsController --> AllLeaveReasonsService

LeaveReasonService --> LeaveReasonInfo
AllLeaveReasonsService --> AllLeaveReasons

LeaveReasonInfo --> LeaveReasonRecord
LeaveReasonRecord --> AppDbContext

@enduml
