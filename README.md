export const getPersons = async () => {
  const res = await fetch("http://localhost:5175/api/Persons/MissingLeaveReasons");
  if (!res.ok) {
    throw new Error("Ошибка загрузки данных");
  }
  return await res.json();
};


import { useEffect, useState } from "react";
import { getPersons } from "../api/personsApi";

export default function PersonsList() {
  const [data, setData] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState("");

  useEffect(() => {
    getPersons()
      .then(setData)
      .catch((e) => setError(e.message))
      .finally(() => setLoading(false));
  }, []);

  if (loading) return <p>Загрузка...</p>;
  if (error) return <p style={{ color: "red" }}>Ошибка: {error}</p>;

  return (
    <div>
      <h2>Сотрудники без причины увольнения</h2>
      <table border="1" cellPadding="6">
        <thead>
          <tr>
            <th>#</th>
            <th>Person ID</th>
            <th>Дата увольнения</th>
            <th>Имя</th>
          </tr>
        </thead>
        <tbody>
          {data.map((person, i) => (
            <tr key={person.personId}>
              <td>{i + 1}</td>
              <td>{person.personId}</td>
              <td>{person.actualTerminationDate}</td>
              <td>{person.fullName}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}



import PersonsList from "./components/PersonsList";

function App() {
  return (
    <div style={{ padding: "20px" }}>
      <h1>Leave Reasons System</h1>
      <PersonsList />
    </div>
  );
}

export default App;


import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);



<!doctype html>
<html lang="ru">
  <head>
    <meta charset="UTF-8" />
    <title>Leave Reasons</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
