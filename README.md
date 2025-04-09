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

class AllLeaveReasons {
  +int RowNumber
  +int ID
  +string Name
  +string NameLocal
  +string LeaveType
  +string LeaveTypeLocal
}

' ==== DbContexts ====
class LeaveReasonDbContext {
  +DbSet<LeaveReasonInfo> LeaveReasonResults
  +Task<List<LeaveReasonInfo>> GetLeaveReasonFromProcedureAsync()
  +Task<bool> UpdateLeaveReasonAsync(int PersonId, string IdLeaveReason)
}

class AllLeaveReasonsDbContext {
  +DbSet<AllLeaveReasons> AllLeaveReasonsResults
  +Task<List<AllLeaveReasons>> GetAllLeaveReasonsFromProcedureAsync()
}

' ==== Services ====
class LeaveReasonService {
  -LeaveReasonDbContext _context
  +Task<List<LeaveReasonInfo>> GetAllPersonsAsync()
  +Task<bool> PutLeaveReasonAsync(LeaveReasonRecord request)
}

class AllLeaveReasonsService {
  -AllLeaveReasonsDbContext _context
  +Task<List<AllLeaveReasons>> GetAllLeaveReasonsAsync()
}

' ==== Controllers ====
class MissingLeaveReasonController {
  -LeaveReasonService _LeaveReasonService
  -ILogger _logger
  +Task<ActionResult<List<LeaveReasonInfo>>> GetLeaveReasonAsync()
  +Task<IActionResult> PutLeaveReasonAsync(LeaveReasonRecord request)
}

class AllLeaveReasonsController {
  -AllLeaveReasonsService _AllLeaveReasonsService
  -ILogger _logger
  +Task<ActionResult<List<AllLeaveReasons>>> GetAllLeaveReasonsAsync()
}

' ==== Relationships ====
LeaveReasonService --> LeaveReasonDbContext
AllLeaveReasonsService --> AllLeaveReasonsDbContext

MissingLeaveReasonController --> LeaveReasonService
AllLeaveReasonsController --> AllLeaveReasonsService

LeaveReasonDbContext --> LeaveReasonInfo
AllLeaveReasonsDbContext --> AllLeaveReasons

LeaveReasonService --> LeaveReasonRecord

@enduml
