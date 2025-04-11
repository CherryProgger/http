/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
};




@tailwind base;
@tailwind components;
@tailwind utilities;

/* Ant Design override (пример синего цвета) */
.ant-btn-primary {
  background-color: #1677ff !important;
}



import axios from "axios";

const BASE_URL = "http://localhost:5175/api/Persons";

export const getPersons = async () => {
  const res = await axios.get(`${BASE_URL}/MissingLeaveReasons`);
  return res.data;
};



import { Table, Button } from "antd";
import { useEffect, useState } from "react";
import { getPersons } from "../api/personsApi";

const columns = [
  {
    title: "№",
    dataIndex: "index",
    key: "index",
    render: (_, __, i) => i + 1,
  },
  {
    title: "Person ID",
    dataIndex: "personId",
    key: "personId",
  },
  {
    title: "Дата увольнения",
    dataIndex: "terminationDate",
    key: "terminationDate",
  },
  {
    title: "Имя",
    dataIndex: "fullName",
    key: "fullName",
  },
  {
    title: "",
    key: "action",
    render: (_, record) => (
      <Button type="primary" onClick={() => console.log(record)}>
        Добавить причину
      </Button>
    ),
  },
];

export default function PersonsTable() {
  const [data, setData] = useState([]);

  useEffect(() => {
    getPersons()
      .then((res) => setData(res))
      .catch((err) => console.error("Ошибка загрузки:", err));
  }, []);

  return (
    <div className="p-8">
      <h1 className="text-2xl font-bold text-blue-700 mb-4">
        Список сотрудников без причины увольнения
      </h1>
      <Table rowKey="personId" columns={columns} dataSource={data} bordered />
    </div>
  );
}



import PersonsTable from "./components/PersonsTable";

function App() {
  return (
    <div className="min-h-screen bg-blue-50">
      <header className="bg-blue-700 text-white px-6 py-4 text-xl font-semibold shadow">
        <a href="/" className="hover:underline">Leave Reasons</a>
      </header>
      <main>
        <PersonsTable />
      </main>
    </div>
  );
}

export default App;





import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import "./index.css";
import "antd/dist/reset.css";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);




<!doctype html>
<html lang="ru">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Leave Reasons</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>




builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowFrontend",
        policy => policy
            .WithOrigins("http://localhost:5173") // или порт твоего фронта
            .AllowAnyMethod()
            .AllowAnyHeader());
});

app.UseCors("AllowFrontend");

using Microsoft.EntityFrameworkCore;
using Microsoft.OpenApi.Models;
using LeaveReasonsSystem.Data;
using LeaveReasonsSystem.Services;

var builder = WebApplication.CreateBuilder(args);

// ✅ 1. DbContext
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

// ✅ 2. Services
builder.Services.AddScoped<PersonsLeaveReasonService>();
builder.Services.AddScoped<LeaveReasonsService>();

// ✅ 3. Controllers
builder.Services.AddControllers();

// ✅ 4. Swagger
builder.Services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new OpenApiInfo { Title = "LeaveReasonService", Version = "v1" });
});

// ✅ 5. CORS (важно: ДО builder.Build())
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowFrontend",
        policy => policy
            .WithOrigins("http://localhost:5173")
            .AllowAnyMethod()
            .AllowAnyHeader());
});

// ✅ 6. Build
var app = builder.Build();

// ✅ 7. Dev environment
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI(c =>
    {
        c.SwaggerEndpoint("/swagger/v1/swagger.json", "LeaveReasonService v1");
    });
}

// ✅ 8. Middleware
app.UseCors("AllowFrontend");
app.UseHttpsRedirection();
app.UseAuthorization();
app.MapControllers();

app.Run();
