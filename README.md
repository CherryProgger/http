.vtb-table-row:hover {
  background-color: #e6f0fa !important; /* Светло-синий ховер */
}

.vtb-table .ant-table-thead > tr > th {
  background-color: #004785;
  color: white;
  font-weight: bold;
}




// App.jsx (главный компонент приложения с Header и стилизацией таблицы в цветах ВТБ)

import React, { useState, useEffect } from "react";
import { Table, Button, Modal, Select, message, Layout } from "antd";
import { getPersons, getLeaveReasons, saveLeaveReasons } from "./api/leaveReasonsApi";

const { Option } = Select;
const { Header, Content } = Layout;

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
            <Button type="primary" onClick={() => setSelectedPerson(person)}>Добавить</Button>
          )}
        </>
      ),
    },
  ];

  return (
    <Layout>
      <Header style={{ background: '#004785', color: 'white', fontSize: '24px', padding: '0 20px' }}>
        Leave Reasons
      </Header>

      <Content style={{ padding: 20 }}>
        <Table
          dataSource={persons}
          rowKey="personId"
          columns={columns}
          pagination={false}
          bordered
          style={{ background: 'white', borderRadius: '8px', overflow: 'hidden' }}
          rowClassName={() => 'vtb-table-row'}
          className="vtb-table"
        />

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
      </Content>
    </Layout>
  );
}
