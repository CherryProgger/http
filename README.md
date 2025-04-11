const handleSave = async () => {
  setConfirmVisible(false);
  try {
    const dataToSave = Object.entries(changes).map(([personId, idLeaveReason]) => ({
      personId: parseInt(personId),
      idLeaveReason,
    }));

    await saveLeaveReasons(dataToSave);
    message.success(lang === "ru" ? "Успешно сохранено" : "Successfully saved");
  } catch (e) {
    message.error(e.message);
  } finally {
    setChanges({});          // ✅ Очистить все локальные изменения
    setSelectedPerson(null); // ✅ Закрыть модалку, если вдруг осталась открыта
    setSearchQuery("");      // ✅ Очистить поле поиска
    await loadData();        // 🔄 Обновить данные
  }
};
