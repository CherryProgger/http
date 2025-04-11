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
    window.location.reload(); // жёсткая перезагрузка — точно удаляет обновлённые строки
  } catch (e) {
    message.error(e.message);
  }
};
