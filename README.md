
// Основной каркас приложения Leave Reasons System

import React, { useState, useEffect } from "react";
import { Table, Button, Modal, Select, message } from "antd";

const { Option } = Select;

// API функции
const getPersons = async () => {
  const res = await fetch("http://localhost:5175/api/Persons/MissingLeaveReasons");
  if (!res.ok) throw new Error("Ошибка загрузки сотрудников");
  return res.json();
};

const getLeaveReasons = async () => {
  const res = await fetch("http://localhost:5175/api/LeaveReasons");
  if (!res.ok) throw new Error("Ошибка загрузки справочника причин");
  return res.json();
};

const saveLeaveReasons = async (data) => {
  const res = await fetch("http://localhost:5175/api/Persons/MissingLeaveReasons", {
    method: "PUT",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(data),
  });
  if (!res.ok) throw new Error("Ошибка сохранения данных");
  return res.json();
};

export default function App() {
  const [persons, setPersons] = useState([]);
  const [leaveReasons, setLeaveReasons] = useState([]);
  const [selectedPerson, setSelectedPerson] = useState(null);
  const [filterType, setFilterType] = useState("Все");
  const [changes, setChanges] = useState({});
  const [confirmVisible, setConfirmVisible] = useState(false);

  useEffect(() => {
    loadData();
  }, []);

  const loadData = async () => {
    try {
      setPersons(await getPersons());
      setLeaveReasons(await getLeaveReasons());
    } catch (e) {
      message.error(e.message);
    }
  };

  const handleSave = async () => {
    setConfirmVisible(false);
    try {
      const dataToSave = Object.entries(changes).map(([personId, idLeaveReason]) => ({
        personId: parseInt(personId),
        idLeaveReason,
      }));
      await saveLeaveReasons(dataToSave);
      message.success("Успешно сохранено");
      loadData();
      setChanges({});
    } catch (e) {
      message.error(e.message);
    }
  };

  const filteredReasons = leaveReasons.filter(
    (r) => filterType === "Все" || r.leaveType === filterType
  );

  const columns = [
    { title: "ID", dataIndex: "personId" },
    { title: "Дата увольнения", dataIndex: "actualTerminationDate" },
    { title: "Имя", dataIndex: "fullName" },
    {
      title: "Причина",
      render: (_, person) => (
        <>
          {changes[person.personId] ? (
            <span>
              {leaveReasons.find((r) => r.id === changes[person.personId])?.nameLocal || "Выбрано"}
            </span>
          ) : (
            <Button onClick={() => setSelectedPerson(person)}>Добавить</Button>
          )}
        </>
      ),
    },
  ];

  return (
    <div style={{ padding: 20 }}>
      <h1>Leave Reasons System</h1>
      <Table dataSource={persons} rowKey="personId" columns={columns} pagination={false} />

      {Object.keys(changes).length > 0 && (
        <Button type="primary" onClick={() => setConfirmVisible(true)} style={{ marginTop: 20 }}>
          Сохранить изменения
        </Button>
      )}

      <Modal
        title="Выбор причины увольнения"
        open={!!selectedPerson}
        onCancel={() => setSelectedPerson(null)}
        footer={null}
      >
        <Select
          style={{ width: "100%", marginBottom: 10 }}
          value={filterType}
          onChange={(value) => setFilterType(value)}
        >
          <Option value="Все">Все</Option>
          <Option value="Involuntary">Involuntary</Option>
          <Option value="Voluntary">Voluntary</Option>
          <Option value="Transfer">Transfer</Option>
        </Select>

        {filteredReasons.map((reason) => (
          <div key={reason.id} style={{ marginBottom: 8, cursor: "pointer" }}
            onClick={() => {
              setChanges({ ...changes, [selectedPerson.personId]: reason.id });
              setSelectedPerson(null);
            }}>
            {reason.nameLocal}
          </div>
        ))}
      </Modal>

      <Modal
        title="Подтверждение сохранения"
        open={confirmVisible}
        onCancel={() => setConfirmVisible(false)}
        onOk={handleSave}
      >
        <p>Вы уверены, что хотите сохранить изменения?</p>
      </Modal>
    </div>
  );
}
