// src/pages/Home.jsx
import { useEffect, useState } from "react";
import EmployeeCard from "../components/EmployeeCard";

export default function Home() {
  const [employees, setEmployees] = useState([]);

  useEffect(() => {
    fetch("/api/LeaveReason/persons")
      .then((res) => res.json())
      .then((data) => setEmployees(data))
      .catch((err) => console.error("Ошибка загрузки:", err));
  }, []);

  return (
    <div className="min-h-screen bg-gray-100 py-10 px-6">
      <h1 className="text-3xl font-bold text-center mb-8">Список сотрудников</h1>
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        {employees.map((emp, index) => (
          <EmployeeCard key={index} employee={emp} />
        ))}
      </div>
    </div>
  );
}


index.css
@tailwind base;
@tailwind components;
@tailwind utilities;



tailwind.config.js
/** @type {import('tailwindcss').Config} */
export default {
  content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
}
