import React, { useState, useEffect } from "react";
import { Table, Button, Modal, Select, message, Layout, Switch } from "antd";
import { getPersons, getLeaveReasons, saveLeaveReasons } from "./api/leaveReasonsApi";

const { Option } = Select;
const { Header, Content } = Layout;

export default function App() {
  const [persons, setPersons] = useState([]);
  const [leaveReasons, setLeaveReasons] = useState([]);
  const [selectedPerson, setSelectedPerson] = useState(null);
  const [filterType, setFilterType] = useState("Все");
  const [searchQuery, setSearchQuery] = useState("");
  const [changes, setChanges] = useState({});
  const [confirmVisible, setConfirmVisible] = useState(false);
  const [lang, setLang] = useState("ru");

  useEffect(() => {
    loadData();
  }, []);

  const loadData = async () => {
    try {
      const [personsData, reasonsData] = await Promise.all([
        getPersons(),
        getLeaveReasons()
      ]);
      setPersons(personsData);
      setLeaveReasons(reasonsData);
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
      message.success(lang === "ru" ? "Успешно сохранено" : "Successfully saved");
      setChanges({});
      window.location.reload(); // Жёсткая перезагрузка
    } catch (e) {
      message.error(e.message);
    }
  };

  const localizedFilter = {
    ru: {
      all: "Все",
      voluntary: "Добровольное",
      involuntary: "Недобровольное",
      transfer: "Перевод",
    },
    en: {
      all: "All types",
      voluntary: "Voluntary",
      involuntary: "Involuntary",
      transfer: "Transfer",
    },
  };

  const getDefaultFilterValue = () => (lang === "ru" ? "Все" : "All types");

  const filteredReasons = leaveReasons.filter((r) => {
    const matchesType =
      filterType === getDefaultFilterValue() || r.leaveType === filterType;

    const nameToCheck = r[lang === "ru" ? "nameLocal" : "name"]
      .toLowerCase();
    const query = searchQuery.toLowerCase();

    const matchesSearch = nameToCheck.includes(query);

    return matchesType && matchesSearch;
  });

  const columns = [
    { title: "ID", dataIndex: "personId" },
    {
      title: lang === "ru" ? "Дата увольнения" : "Termination Date",
      dataIndex: "ActualTerminationDate",
    },
    {
      title: lang === "ru" ? "ФИО" : "Full Name",
      dataIndex: lang === "ru" ? "fullNameLocal" : "fullName",
    },
    {
      title: lang === "ru" ? "Причина увольнения" : "Reason for Termination",
      render: (_, person) => (
        <>
          {changes[person.personId] ? (
            <span style={{ marginRight: 8 }}>
              {leaveReasons.find((r) => r.id === changes[person.personId])?.[lang === "ru" ? "nameLocal" : "name"] || "Выбрано"}
            </span>
          ) : null}

          <Button
            type="primary"
            onClick={() => {
              setFilterType(getDefaultFilterValue());
              setSearchQuery("");
              setSelectedPerson(person);
            }}
          >
            {changes[person.personId]
              ? lang === "ru" ? "Изменить" : "Edit"
              : lang === "ru" ? "Добавить" : "Add"}
          </Button>
        </>
      ),
    },
  ];

  return (
    <Layout>
      <Header style={{ background: '#e6f0fa', color: '#004785', fontSize: '24px', padding: '0 20px', fontWeight: 'bold', display: 'flex', justifyContent: 'space-between', alignItems: 'center' }}>
        Leave Reasons
        <div>
          <span style={{ marginRight: 8 }}>RU</span>
          <Switch checked={lang === "en"} onChange={(checked) => setLang(checked ? "en" : "ru")} />
          <span style={{ marginLeft: 8 }}>EN</span>
        </div>
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
            {lang === "ru" ? "Сохранить изменения" : "Save changes"}
          </Button>
        )}

        <Modal
          title={lang === "ru" ? "Выбор причины увольнения" : "Select Termination Reason"}
          open={!!selectedPerson}
          onCancel={() => setSelectedPerson(null)}
          footer={null}
        >
          <Select
            style={{ width: "100%", marginBottom: 10 }}
            value={filterType}
            onChange={(value) => setFilterType(value)}
          >
            <Option value={getDefaultFilterValue()}>
              {localizedFilter[lang].all}
            </Option>
            <Option value="Voluntary">{localizedFilter[lang].voluntary}</Option>
            <Option value="Involuntary">{localizedFilter[lang].involuntary}</Option>
            <Option value="Transfer">{localizedFilter[lang].transfer}</Option>
          </Select>

          <input
            type="text"
            placeholder={lang === "ru" ? "Поиск причины..." : "Search reason..."}
            value={searchQuery}
            onChange={(e) => setSearchQuery(e.target.value)}
            style={{ width: "100%", marginBottom: 10, padding: 8, borderRadius: 4, border: "1px solid #ccc" }}
          />

          {filteredReasons.map((reason) => (
            <div
              key={reason.id}
              style={{ marginBottom: 8, cursor: "pointer" }}
              onClick={() => {
                setChanges({ ...changes, [selectedPerson.personId]: reason.id });
                setSelectedPerson(null);
              }}
            >
              {reason[lang === "ru" ? "nameLocal" : "name"]}
            </div>
          ))}
        </Modal>

        <Modal
          title={lang === "ru" ? "Подтверждение сохранения" : "Confirm Save"}
          open={confirmVisible}
          onCancel={() => setConfirmVisible(false)}
          onOk={handleSave}
        >
          <p>
            {lang === "ru"
              ? "Вы уверены, что хотите сохранить изменения?"
              : "Are you sure you want to save the changes?"}
          </p>
        </Modal>
      </Content>
    </Layout>
  );
}
