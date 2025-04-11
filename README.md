@startuml
actor User
participant "App.jsx" as App
participant "leaveReasonsApi.js" as API
participant "Backend API" as Server

== Загрузка данных ==
User -> App : Открывает страницу
App -> API : getPersons()
API -> Server : GET /api/Persons/MissingLeaveReasons
Server --> API : список персон
API --> App : данные персон

App -> API : getLeaveReasons()
API -> Server : GET /api/LeaveReasons
Server --> API : причины увольнения
API --> App : словарь причин

== Выбор причины ==
User -> App : Нажимает "Добавить/Изменить"
App -> User : Показывает модалку выбора

User -> App : Выбирает причину
App -> App : Сохраняет выбранную причину в changes

== Сохранение ==
User -> App : Нажимает "Сохранить изменения"
App -> API : saveLeaveReasons(changes)
API -> Server : PUT /api/Persons/MissingLeaveReasons
Server --> API : Результат
API --> App : Успешно

App -> Browser : window.location.reload()

@enduml







@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Component.puml

LAYOUT_TOP_DOWN()

Person(user, "User", "HR-сотрудник, выбирающий причины увольнения")

System_Boundary(frontend, "React Frontend") {
  Component(App, "App.jsx", "React Component", "Основной компонент приложения")
  Component(LeaveReasonsApi, "leaveReasonsApi.js", "JS-модуль", "Обёртка над REST API")
  Component(AntdTable, "Ant Design Table", "UI-компонент", "Рендерит таблицу персон")
  Component(SwitchLang, "Language Switcher", "UI-компонент", "Позволяет переключить язык интерфейса")
}

System_Ext(api, "Backend API", "REST API")

Rel(user, App, "Использует")
Rel(App, LeaveReasonsApi, "Вызывает")
Rel(LeaveReasonsApi, api, "Запрашивает данные")
Rel(App, AntdTable, "Передаёт данные для рендеринга")
Rel(App, SwitchLang, "Использует для смены языка")

@enduml
