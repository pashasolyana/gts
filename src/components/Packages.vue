<template>
  <v-container fluid>
    <!-- Форма поиска -->
    <v-row no-gutters align="center">
      <!-- Поле для поиска по ФИО -->
      <v-col cols="12" sm="6" md="4" lg="3" class="px-2">
        <div class="input-label">ФИО</div>
        <v-text-field
          v-model="search.fio"
          variant="outlined"
          class="custom-input"
          dense
          hide-details
          append-inner-icon="mdi-magnify"
          @keyup.enter="startSearch"
        ></v-text-field>
      </v-col>

      <!-- Поле для поиска по телефону -->
      <v-col cols="12" sm="6" md="4" lg="3" class="px-2">
        <div class="input-label">Телефон</div>
        <v-text-field
          v-model="search.phone"
          variant="outlined"
          class="custom-input"
          dense
          hide-details
          append-inner-icon="mdi-magnify"
          @keyup.enter="startSearch"
        ></v-text-field>
      </v-col>

      <!-- Поле для поиска по паспорту -->
      <v-col cols="12" sm="6" md="4" lg="3" class="px-2">
        <div class="input-label">Серия и номер паспорта</div>
        <v-text-field
          v-model="search.passport"
          variant="outlined"
          class="custom-input"
          dense
          hide-details
          append-inner-icon="mdi-magnify"
          @keyup.enter="startSearch"
        ></v-text-field>
      </v-col>

      <!-- Кнопка "Очистить" -->
      <v-col cols="12" sm="6" md="2" lg="1" class="px-2">
        <v-btn class="custom-button grey" @click="clearSearch"> Очистить </v-btn>
      </v-col>

      <!-- Кнопка "Найти" -->
      <v-col cols="12" sm="6" md="2" lg="1" class="px-2">
        <v-btn class="custom-button find" @click="startSearch"> Найти </v-btn>
      </v-col>

      <!-- Поле для даты начала -->
      <v-col cols="12" sm="6" md="2" lg="1" class="px-2 mt-4">
        <custom-date-picker v-model="dateRange.start" />
      </v-col>
      <div class="mt-3">-</div>
      <!-- Поле для даты окончания -->
      <v-col cols="12" sm="6" md="2" lg="1" class="px-2 mt-4">
        <custom-date-picker v-model="dateRange.end" />
      </v-col>

      <!-- Кнопка с календарем для выбора диапазона дат -->
      <v-col cols="12" sm="6" md="1" lg="1" class="px-2 mt-1">
        <v-btn icon variant="text" class="custom-icon">
          <img src="../assets/calendar.png" />
        </v-btn>
      </v-col>

      <!-- Иконка выгрузки -->
      <v-col cols="12" sm="6" md="1" lg="1" class="px-2 mt-1">
        <v-btn icon variant="text" class="custom-icon" @click="downloadData">
          <img src="../assets/download.png" />
        </v-btn>
      </v-col>

      <!-- Пользовательская информация -->
      <v-col cols="12" sm="6" md="2" lg="2" class="px-2 user-info">
        <div class="d-flex align-center justify-end">
          <div class="user-name">
            <div class="last-name">Быкова</div>
            <div class="first-name">Наталья</div>
          </div>
          <v-avatar size="36" class="ml-2">
            <img src="../assets/empty-user.png" alt="User Avatar" />
          </v-avatar>
          <v-btn icon variant="text" class="ml-2 settings-icon">
            <img src="../assets/settings.png" />
          </v-btn>
        </div>
      </v-col>
    </v-row>

    <!-- Таблица -->
    <v-row class="mt-6">
      <v-col cols="12">
        <!-- Обертка для таблицы с горизонтальной прокруткой -->
        <div class="table-wrapper">
          <div class="table-container">
            <!-- Заголовок таблицы -->
            <v-row no-gutters class="table-header">
              <v-col cols="12" class="data-col">
                <v-row no-gutters>
                  <v-col class="text-left header-cell ml-4" style="min-width: 200px"
                    >ТК</v-col
                  >
                  <v-col class="text-center header-cell" style="min-width: 140px"
                    >Дата создания</v-col
                  >
                  <v-col class="text-center header-cell" style="min-width: 160px"
                    >Трек номер</v-col
                  >
                  <v-col class="text-center header-cell" style="min-width: 120px"
                    >Статус ТК</v-col
                  >
                  <v-col class="text-center header-cell" style="min-width: 140px"
                    >Дата статуса</v-col
                  >
                  <v-col class="text-center header-cell" style="min-width: 120px"
                    >Сумма выплаты</v-col
                  >
                  <v-col class="text-center header-cell" style="min-width: 120px"
                    >Способ вывода</v-col
                  >
                  <v-col class="text-center header-cell" style="min-width: 180px"
                    >Получатель ДС</v-col
                  >
                  <v-col class="text-center header-cell" style="min-width: 200px"
                    >ФИО</v-col
                  >
                  <v-col class="text-center header-cell" style="min-width: 140px"
                    >Дата вывода</v-col
                  >
                  <v-col class="text-center header-cell" style="min-width: 120px"
                    >Статус</v-col
                  >
                </v-row>
              </v-col>
            </v-row>

            <!-- Строки данных -->
            <div
              v-for="(item, index) in filteredItems"
              :key="index"
              class="table-container"
            >
              <v-row
                no-gutters
                class="table-row"
                :style="{
                  background:
                    item.orderStatus == 'Выдано'
                      ? 'rgba(128, 225, 165, 1)'
                      : item.orderStatus === 'На депозите'
                      ? 'rgba(1, 167, 227, 0.39)'
                      : item.orderStatus === 'Создан'
                      ? 'rgba(205, 205, 205, 1)'
                      : 'rgba(255, 255, 255, 1)',
                }"
              >
                <v-col cols="12" class="data-col">
                  <v-row no-gutters>
                    <v-col class="text-left data-cell" style="min-width: 200px">{{
                      item.name
                    }}</v-col>
                    <v-col class="text-center data-cell" style="min-width: 140px">{{
                      item.date
                    }}</v-col>
                    <v-col
                      class="text-center data-cell"
                      style="min-width: 160px; color: rgba(1, 167, 227, 1)"
                      >{{ item.trekNumber }}</v-col
                    >
                    <v-col class="text-center data-cell" style="min-width: 120px">{{
                      item.statusTk
                    }}</v-col>
                    <v-col class="text-center data-cell" style="min-width: 140px">{{
                      item.statusDate
                    }}</v-col>
                    <v-col
                      class="text-center data-cell"
                      :style="{
                        minWidth: '120px',
                        color:
                          item.orderStatus == 'Не выдано'
                            ? 'rgba(0, 193, 74, 1)'
                            : 'rgba(255, 255, 255, 1)',
                      }"
                      >{{ item.amount }}</v-col
                    >
                    <v-col class="text-center data-cell" style="min-width: 120px">{{
                      item.type
                    }}</v-col>
                    <v-col class="text-center data-cell" style="min-width: 180px">{{
                      item.receiver
                    }}</v-col>
                    <v-col class="text-center data-cell" style="min-width: 200px">{{
                      item.fio
                    }}</v-col>
                    <v-col class="text-center data-cell" style="min-width: 140px">{{
                      item.orderDate
                    }}</v-col>
                    <v-col class="text-center data-cell" style="min-width: 120px">{{
                      item.orderStatus
                    }}</v-col>
                  </v-row>
                </v-col>
              </v-row>
            </div>
          </div>
        </div>
      </v-col>
    </v-row>

    <!-- Модальное окно -->
    <modal-courier ref="modalCourier" :item="selectedItem" @save="saveModalData" />
  </v-container>
</template>

<script>
import CustomDatePicker from "./CustomDatePicker.vue";
import ModalCourier from "./ModalCourier.vue";

export default {
  name: "SearchForm",
  components: {
    CustomDatePicker,
    ModalCourier,
  },
  data() {
    return {
      search: {
        fio: "",
        phone: "",
        passport: "",
      },
      dateRange: {
        start: null,
        end: null,
      },
      dateRangeArray: [],
      rangeMenu: false,
      tableItems: [
        {
          name: "Почта",
          date: "14.02.2025",
          trekNumber: "880099702098183",
          statusTk: "Принят",
          statusDate: "15.02.2025",
          amount: "100 ₽",
          type: "СБП",
          receiver: "+ 7 (999) 888-88-88",
          fio: "Амина Хазбулатовна Х.",
          orderDate: "15.02.2025",
          orderStatus: "Выдано",
        },
        {
          name: "Почта",
          date: "14.02.2025",
          trekNumber: "880099702098183",
          statusTk: "Принят",
          statusDate: "15.02.2025",
          amount: "100 ₽",
          type: "СБП",
          receiver: "+ 7 (999) 888-88-88",
          fio: "Амина Хазбулатовна Х.",
          orderDate: "15.02.2025",
          orderStatus: "На депозите",
        },
        {
          name: "Почта",
          date: "14.02.2025",
          trekNumber: "880099702098183",
          statusTk: "Принят",
          statusDate: "15.02.2025",
          amount: "100 ₽",
          type: "СБП",
          receiver: "+ 7 (999) 888-88-88",
          fio: "Амина Хазбулатовна Х.",
          orderDate: "15.02.2025",
          orderStatus: "Создан",
        },
        {
          name: "Почта",
          date: "14.02.2025",
          trekNumber: "880099702098183",
          statusTk: "Принят",
          statusDate: "15.02.2025",
          amount: "100 ₽",
          type: "СБП",
          receiver: "+ 7 (999) 888-88-88",
          fio: "Амина Хазбулатовна Х.",
          orderDate: "15.02.2025",
          orderStatus: "Не выдано",
        },
      ],
      selectedItem: null,
    };
  },
  computed: {
    filteredItems() {
      let filtered = [...this.tableItems];

      if (this.search.fio) {
        filtered = filtered.filter((item) =>
          item.fio.toLowerCase().includes(this.search.fio.toLowerCase())
        );
      }

      if (this.search.phone) {
        filtered = filtered.filter((item) => item.phone.includes(this.search.phone));
      }

      if (this.search.passport) {
        filtered = filtered.filter((item) =>
          item.passport.includes(this.search.passport)
        );
      }

      if (this.dateRange.start && this.dateRange.end) {
        const startDate = new Date(this.dateRange.start);
        const endDate = new Date(this.dateRange.end);
        filtered = filtered.filter((item) => {
          const itemDate = new Date(item.birthDate);
          return itemDate >= startDate && itemDate <= endDate;
        });
      }

      return filtered;
    },
  },
  methods: {
    clearSearch() {
      this.search.fio = "";
      this.search.phone = "";
      this.search.passport = "";
      this.dateRange.start = null;
      this.dateRange.end = null;
      this.dateRangeArray = [];
    },
    startSearch() {
      console.log("Search started with:", {
        fio: this.search.fio,
        phone: this.search.phone,
        passport: this.search.passport,
        dateRange: this.dateRange,
      });
    },
    downloadData() {
      console.log("Download triggered");
    },
    deleteItem(item) {
      console.log("Delete item:", item);
      this.tableItems = this.tableItems.filter((i) => i !== item);
    },
    updateDateRange() {
      if (this.dateRangeArray.length === 2) {
        const [start, end] = this.dateRangeArray.sort();
        this.dateRange.start = start;
        this.dateRange.end = end;
        this.rangeMenu = false;
      }
    },
    formatDate(date) {
      if (!date) return "";
      const [year, month, day] = date.split("-");
      return `${day.padStart(2, "0")}.${month.padStart(2, "0")}.${year}`;
    },
    openModal(item) {
      console.log("Opening modal for item:", item);
      this.selectedItem = { ...item };
      this.$refs.modalCourier.openModal();
    },
    saveModalData(formData) {
      console.log("Saving modal data:", formData);
      const index = this.tableItems.findIndex((item) => item === this.selectedItem);
      if (index !== -1) {
        this.tableItems[index] = {
          ...this.tableItems[index],
          ...formData,
          birthDate: formData.birthDate
            ? this.parseDate(formData.birthDate)
            : this.tableItems[index].birthDate,
        };
      }
      this.selectedItem = null;
    },
    parseDate(dateStr) {
      const [day, month, year] = dateStr.split(".");
      return `${year}-${month.padStart(2, "0")}-${day.padStart(2, "0")}`;
    },
  },
};
</script>

<style scoped>
/* Стили для кастомного лейбла */
.input-label {
  font-size: 10px;
  color: rgba(0, 0, 0, 1);
  text-align: left;
}

.grey {
  background-color: rgba(0, 0, 0, 0.25);
  font-size: 10px;
  margin-top: 1.5vh;
}

.find {
  background-color: rgba(1, 167, 227, 1);
  font-size: 10px;
  margin-top: 1.5vh;
  color: white;
}

/* Удаляем стандартный бордер Vuetify и применяем кастомный для custom-input */
:deep(.custom-input .v-field__outline) {
  display: none !important;
}

:deep(.custom-input .v-field) {
  border: 1px solid rgba(1, 167, 227, 1) !important;
  border-radius: 10px !important;
  box-shadow: none !important;
  background: transparent !important;
  min-height: 31px !important;
  display: flex !important;
  align-items: center !important;
}

:deep(.custom-date-picker .date-input) {
  min-height: 31px !important;
  border: 1px solid #01a7e3 !important;
  border-radius: 10px !important;
  box-shadow: none !important;
  background: transparent !important;
  padding: 0 8px !important;
  font-size: 14px !important;
}

:deep(.custom-date-picker) {
  width: 100%;
}

/* Стили для иконки в custom-input */
:deep(.custom-input .v-field__append-inner) {
  align-self: center !important;
  padding-right: 4px !important;
  margin-left: auto !important;
}

:deep(.custom-input .v-field__append-inner .v-icon) {
  font-size: 16px !important;
}

.custom-date-input {
  margin-top: 1.5vh;
}

/* Удаляем стандартный бордер Vuetify и применяем кастомный для custom-date-input */
:deep(.custom-date-input .v-field__outline) {
  display: none !important;
}

:deep(.custom-date-input .v-field) {
  border: 1px solid rgba(1, 167, 227, 1) !important;
  border-radius: 10px !important;
  box-shadow: none !important;
  background: transparent !important;
  min-height: 31px !important;
  display: flex !important;
  align-items: center !important;
}

/* Стили для иконки в custom-date-input */
:deep(.custom-date-input .v-field__append-inner) {
  align-self: center !important;
  padding-right: 4px !important;
  margin-left: auto !important;
}

:deep(.custom-date-input .v-field__append-inner .v-icon) {
  font-size: 16px !important;
}

/* Стили для кнопок */
.custom-button {
  border-radius: 10px !important;
  height: 31px !important;
  min-width: 80px !important;
  text-transform: none !important;
}

/* Стили для иконки выгрузки */
.custom-icon {
  border-radius: 10px !important;
  height: 28px !important;
  width: 28px !important;
  margin-top: 1vh;
}

.custom-icon img {
  width: 20px !important;
  height: 20px !important;
}

/* Стили для разделителя дат */
.mt-3 {
  font-size: 16px;
  color: rgba(0, 0, 0, 1);
  margin: 0 4px;
  margin-top: 1.5vh;
}

/* Стили для пользовательской информации */
.user-info {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  margin-top: 1.5vh;
}

.user-name {
  text-align: right;
}

.last-name,
.first-name {
  font-size: 10px;
  color: rgba(0, 0, 0, 1);
  line-height: 1.2;
}

.settings-icon {
  margin-left: 8px !important;
}

.settings-icon img {
  width: 24px !important;
  height: 24px !important;
}

/* Для корневого элемента можно задать дополнительные стили */
.custom-input,
.custom-date-input {
  border-radius: 10px !important;
}

/* Принудительно задаем горизонтальное расположение с переносом */
.v-row {
  width: 100%;
  display: flex;
  align-items: center;
}

/* Убедимся, что колонки занимают нужную ширину */
.v-col-md-4 {
  flex: 0 0 auto !important;
  width: auto !important;
}

.v-col-md-2 {
  flex: 0 0 auto !important;
  width: auto !important;
}

.v-col-md-1 {
  flex: 0 0 auto !important;
  width: auto !important;
}

.v-col-lg-3 {
  flex: 0 0 auto !important;
  width: auto !important;
}

.v-col-lg-2 {
  flex: 0 0 auto !important;
  width: auto !important;
}

.v-col-lg-1 {
  flex: 0 0 auto !important;
  width: auto !important;
}

/* Дополнительные стили для предотвращения наложения */
:deep(.v-field) {
  min-height: 31px !important;
  padding: 0 8px !important;
}

/* Отступы между полями */
.px-2 {
  padding-left: 8px !important;
  padding-right: 8px !important;
}

/* Стили для таблицы */
.table-wrapper {
  overflow-x: auto;
  width: 100%;
}

.table-container {
  min-width: 1400px; /* Минимальная ширина таблицы */
  width: 100%;
}

.table-header {
  background-color: #00a7e3;
  border-radius: 10px 10px 0 0;
  padding: 10px 0;
  min-height: 50px;
  align-items: center;
}

.data-col {
  flex: 0 0 100%;
}

.header-cell {
  color: white;
  font-size: 12px;
  padding: 8px 12px;
  font-weight: 500;
}

/* Выравнивание заголовка ФИО по левому краю */
.header-cell:first-child {
  text-align: left;
  padding-left: 24px;
}

.table-row {
  background-color: #ffffff;
  border-bottom: 2px solid #e0e0e0;
  padding: 8px 0;
  margin-bottom: 15px !important;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  min-height: 50px;
  align-items: center;
}

.table-row:last-child {
  border-bottom: none;
  margin-bottom: 0;
}

.data-cell {
  color: #000000;
  font-size: 13px;
  padding: 8px 12px;
}

/* Специфические стили для столбца ФИО */
.data-cell:first-child {
  white-space: nowrap;
  text-overflow: ellipsis;
  text-align: left;
  padding-left: 24px;
}

/* Медиа-запрос для экрана 1920x1080 */
@media screen and (min-width: 1920px) and (max-width: 1920px) {
  :deep(.custom-input .v-field) {
    width: 201px !important;
    min-width: 201px !important;
    max-width: 201px !important;
    height: 31px !important;
    min-height: 31px !important;
    max-height: 31px !important;
  }

  :deep(.custom-date-input .v-field) {
    width: 109px !important;
    min-width: 109px !important;
    max-width: 109px !important;
    height: 31px !important;
    min-height: 31px !important;
    max-height: 31px !important;
  }

  .custom-button {
    width: 80px !important;
    min-width: 80px !important;
    max-width: 80px !important;
  }

  .custom-icon {
    width: 31px !important;
    min-width: 31px !important;
    max-width: 31px !important;
  }

  .user-info {
    margin-left: auto !important;
    margin-top: 1.5vh !important;
  }
}

/* Медиа-запрос для больших экранов (выше 1920px) */
@media screen and (min-width: 1921px) {
  :deep(.custom-input .v-field) {
    width: 201px !important;
    min-width: 201px !important;
    max-width: 201px !important;
    height: 31px !important;
  }

  :deep(.custom-date-input .v-field) {
    width: 109px !important;
    min-width: 109px !important;
    max-width: 109px !important;
    height: 31px !important;
  }

  .custom-button {
    width: 80px !important;
    min-width: 80px !important;
    max-width: 80px !important;
  }

  .custom-icon {
    width: 31px !important;
    min-width: 31px !important;
    max-width: 31px !important;
  }

  .user-info {
    margin-left: auto !important;
    margin-top: 1.5vh !important;
  }
}

/* Адаптивность для средних экранов (960px - 1919px) */
@media screen and (min-width: 960px) and (max-width: 1919px) {
  :deep(.custom-input .v-field) {
    width: 180px !important;
    min-width: 180px !important;
    max-width: 180px !important;
    height: 31px !important;
  }

  :deep(.custom-date-input .v-field) {
    width: 90px !important;
    min-width: 90px !important;
    max-width: 90px !important;
    height: 31px !important;
  }

  .custom-button {
    width: 70px !important;
    min-width: 70px !important;
    max-width: 70px !important;
  }

  .custom-icon {
    width: 28px !important;
    min-width: 28px !important;
    max-width: 28px !important;
  }

  .user-info {
    margin-left: auto !important;
    margin-top: 1.5vh !important;
  }
}

/* Адаптивность для меньших экранов (меньше 960px) */
@media screen and (max-width: 959px) {
  .v-col-md-4,
  .v-col-md-2,
  .v-col-md-1,
  .v-col-lg-3,
  .v-col-lg-2,
  .v-col-lg-1 {
    flex: 0 0 100% !important;
    max-width: 100% !important;
  }

  .user-info {
    justify-content: center !important;
    margin-top: 1.5vh !important;
  }

  :deep(.custom-input .v-field),
  :deep(.custom-date-input .v-field) {
    width: 100% !important;
    max-width: 100% !important;
  }

  .custom-button {
    width: 100% !important;
    max-width: 100% !important;
  }

  .custom-icon {
    width: 31px !important;
    min-width: 31px !important;
    max-width: 31px !important;
  }

  .mt-3 {
    margin-top: 0.5vh !important;
  }

  .table-container {
    min-width: 100%; /* На маленьких экранах таблица занимает всю ширину */
  }

  .data-cell {
    width: 100% !important;
    max-width: 100% !important;
  }

  .data-cell:first-child {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    text-align: left;
    padding-left: 16px;
  }
}
</style>
