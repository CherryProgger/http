// 1. src/index.js
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import './index.css';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);


// 2. src/index.css
@tailwind base;
@tailwind components;
@tailwind utilities;

body {
  @apply bg-[#f0f5ff] text-[#003a8c] font-sans;
}

a {
  @apply text-[#1890ff] hover:underline;
}

.ant-btn-primary {
  @apply bg-[#003a8c] border-none hover:bg-[#0050b3];
}


// 3. src/App.js
import React from 'react';
import { Layout } from 'antd';
import HeaderBar from './components/HeaderBar';
import EmployeeTable from './components/EmployeeTable';

const { Content } = Layout;

function App() {
  return (
    <Layout className="min-h-screen">
      <HeaderBar />
      <Content className="p-6">
        <EmployeeTable />
      </Content>
    </Layout>
  );
}

export default App;


// 4. src/components/HeaderBar.js
import React from 'react';
import { Layout } from 'antd';

const { Header } = Layout;

function HeaderBar() {
  return (
    <Header className="bg-[#001529] px-6">
      <a href="/" className="text-white text-xl font-bold">
        Leave Reasons
      </a>
    </Header>
  );
}

export default HeaderBar;


// 5. src/components/EmployeeTable.js
import React, { useEffect, useState } from 'react';
import { Table, Button, Spin, message } from 'antd';
import axios from 'axios';

function EmployeeTable() {
  const [data, setData] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    axios.get('/api/LeaveReason/persons')
      .then((res) => {
        setData(res.data);
      })
      .catch(() => {
        message.error('Ошибка при загрузке данных');
      })
      .finally(() => setLoading(false));
  }, []);

  const columns = [
    {
      title: '№',
      dataIndex: 'rowNumber',
      key: 'rowNumber'
    },
    {
      title: 'ID',
      dataIndex: 'personId',
      key: 'personId'
    },
    {
      title: 'Дата увольнения',
      dataIndex: 'actualTerminationDateFormatted',
      key: 'actualTerminationDateFormatted'
    },
    {
      title: 'ФИО',
      dataIndex: 'fullName',
      key: 'fullName'
    },
    {
      title: '',
      key: 'action',
      render: (_, record) => (
        <Button type="primary" onClick={() => handleAddReason(record)}>
          Добавить причину
        </Button>
      )
    }
  ];

  const handleAddReason = (employee) => {
    message.info(`Добавление причины для ${employee.fullName}`);
    // TODO: открыть модалку
  };

  return (
    <Spin spinning={loading}>
      <Table
        dataSource={data}
        columns={columns}
        rowKey="personId"
        bordered
        pagination={{ pageSize: 10 }}
      />
    </Spin>
  );
}

export default EmployeeTable;








Programm.CS
var builder = WebApplication.CreateBuilder(args);

// Add services
builder.Services.AddControllers();
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowFrontend", policy =>
    {
        policy.WithOrigins("http://localhost:3000")
              .AllowAnyHeader()
              .AllowAnyMethod();
    });
});

// Add your custom services (DI)
builder.Services.AddScoped<ILeaveReasonService, LeaveReasonService>();
builder.Services.AddDbContext<LeaveReasonDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

var app = builder.Build();

app.UseCors("AllowFrontend");
app.UseRouting();
app.UseAuthorization();
app.MapControllers();

app.Run();
