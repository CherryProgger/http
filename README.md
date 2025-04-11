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

<Input
  placeholder={lang === "ru" ? "Поиск причины..." : "Search reason..."}
  value={searchQuery}
  onChange={(e) => setSearchQuery(e.target.value)}
  style={{ marginBottom: 10 }}
/>




import { Input } from "antd";
