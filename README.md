
@startuml
left to right direction
skinparam packageStyle rectangle

actor HRUser as "HR Сотрудник"

rectangle "Leave Reasons App" {
  
  (Получить список сотрудников с причинами увольнения) as UC1
  (Получить все причины увольнений) as UC2
  (Изменить причину увольнения сотрудника) as UC3
  
  HRUser --> UC1
  HRUser --> UC2
  HRUser --> UC3
}

@enduml
