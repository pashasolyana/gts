<template>
  <v-dialog v-model="dialog" max-width="800" persistent>
    <v-card class="modal-card">
      <v-card-title class="modal-title">Карточка Курьера</v-card-title>
      <v-card-text>
        <!-- Персональные данные -->
        <v-row no-gutters align="start" class="modal-section">
          <v-col cols="12">
            <div class="input-label section-title">Персональные данные</div>
          </v-col>
          <v-col cols="12">
            <v-row no-gutters>
              <v-col cols="12" sm="4" class="px-2">
                <v-text-field
                  v-model="form.fio"
                  placeholder="Быкова Александра"
                  variant="outlined"
                  dense
                  hide-details
                  class="custom-input"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="4" class="px-2">
                <v-text-field
                  v-model="form.phone"
                  placeholder="+7968333333"
                  variant="outlined"
                  dense
                  hide-details
                  class="custom-input"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="4" class="px-2">
                <v-text-field
                  v-model="form.abdjarim"
                  placeholder="Абджарим"
                  variant="outlined"
                  dense
                  hide-details
                  class="custom-input"
                ></v-text-field>
              </v-col>
            </v-row>
            <v-row no-gutters class="mt-2">
              <v-col cols="12" sm="4" class="px-2">
                <v-select
                  v-model="form.passportType"
                  :items="passportOptions"
                  placeholder="Дополнительное поле"
                  variant="outlined"
                  dense
                  hide-details
                  class="custom-input"
                ></v-select>
              </v-col>
              <v-col cols="12" sm="4" class="px-2">
                <v-text-field
                  v-model="form.passportNumber"
                  placeholder="4008858887"
                  variant="outlined"
                  dense
                  hide-details
                  class="custom-input"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="4" class="px-2">
                <v-text-field
                  v-model="form.birthDate"
                  placeholder="22.02.2025"
                  variant="outlined"
                  dense
                  hide-details
                  class="custom-input"
                  readonly
                  @click="openDatePicker"
                >
                  <template v-slot:append-inner>
                    <v-icon @click="openDatePicker">mdi-calendar</v-icon>
                  </template>
                </v-text-field>
              </v-col>
            </v-row>
            <v-row no-gutters class="mt-2">
              <v-col cols="12" sm="4" class="px-2">
                <v-text-field
                  v-model="form.extraField"
                  placeholder="Дополнительное поле"
                  variant="outlined"
                  dense
                  hide-details
                  class="custom-input"
                ></v-text-field>
              </v-col>
            </v-row>
          </v-col>
        </v-row>

        <!-- Ограничение на кол-во посылок -->
        <v-row no-gutters align="start" class="modal-section mt-4">
          <v-col cols="12">
            <div class="input-label section-title">Ограничение на кол-во посылок</div>
          </v-col>
          <v-col cols="12">
            <v-row no-gutters class="package-row">
              <v-col cols="12" sm="2" class="px-2">
                <v-text-field
                  value="Сутки"
                  variant="outlined"
                  dense
                  hide-details
                  readonly
                  class="static-input"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="2" class="px-2 package-gap">
                <v-text-field
                  v-model="form.packagesSutki"
                  placeholder="300 шт."
                  variant="outlined"
                  dense
                  hide-details
                  class="custom-input"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="2" class="px-2">
                <v-text-field
                  value="Неделя"
                  variant="outlined"
                  dense
                  hide-details
                  readonly
                  class="static-input"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="2" class="px-2 package-gap">
                <v-text-field
                  v-model="form.packagesNedelia"
                  placeholder="2000 шт."
                  variant="outlined"
                  dense
                  hide-details
                  class="custom-input"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="2" class="px-2">
                <v-text-field
                  value="Месяц"
                  variant="outlined"
                  dense
                  hide-details
                  readonly
                  class="static-input"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="2" class="px-2 package-gap">
                <v-text-field
                  v-model="form.packagesMesiac"
                  placeholder="5000 шт."
                  variant="outlined"
                  dense
                  hide-details
                  class="custom-input"
                ></v-text-field>
              </v-col>
            </v-row>
          </v-col>
          <v-col cols="12">
            <div
              class="input-label section-title"
              :style="{
                marginTop: '8px',
                marginBottom: '8px',
              }"
            >
              Ограничение на сумму сданных посылок
            </div>
          </v-col>
          <v-col cols="12">
            <v-row no-gutters class="package-row mt-2">
              <v-col cols="12" sm="2" class="px-2">
                <v-text-field
                  value="Сутки"
                  variant="outlined"
                  dense
                  hide-details
                  readonly
                  class="static-input"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="2" class="px-2 package-gap">
                <v-text-field
                  v-model="form.moneySutki"
                  placeholder="20 000,00 ₽"
                  variant="outlined"
                  dense
                  hide-details
                  class="custom-input"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="2" class="px-2">
                <v-text-field
                  value="Неделя"
                  variant="outlined"
                  dense
                  hide-details
                  readonly
                  class="static-input"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="2" class="px-2 package-gap">
                <v-text-field
                  v-model="form.moneyNedelia"
                  placeholder="150 000,00 ₽"
                  variant="outlined"
                  dense
                  hide-details
                  class="custom-input"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="2" class="px-2">
                <v-text-field
                  value="Месяц"
                  variant="outlined"
                  dense
                  hide-details
                  readonly
                  class="static-input"
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="2" class="px-2 package-gap">
                <v-text-field
                  v-model="form.moneyMesiac"
                  placeholder="600 000,00 ₽"
                  variant="outlined"
                  dense
                  hide-details
                  class="custom-input"
                ></v-text-field>
              </v-col>
            </v-row>
          </v-col>
        </v-row>

        <!-- Вознаграждение за посылку по ТК -->
        <v-row no-gutters align="start" class="modal-section mt-4">
          <v-col cols="12">
            <div class="input-label section-title">Вознаграждение за посылку по ТК</div>
          </v-col>
          <v-col cols="12">
            <v-row no-gutters>
              <v-col
                cols="12"
                sm="2"
                class="px-2 mb-4"
                v-for="tk in transportCompanies.slice(0, 6)"
                :key="tk"
              >
                <div class="label-above">{{ tk }}</div>
                <v-text-field
                  v-model="form[tk.toLowerCase().replace(/[\.\s]/g, '')]"
                  placeholder="100,00 ₽"
                  variant="outlined"
                  dense
                  hide-details
                  class="custom-input"
                ></v-text-field>
              </v-col>
            </v-row>
            <v-row no-gutters class="mt-2">
              <v-col
                cols="12"
                sm="2"
                class="px-2"
                v-for="tk in transportCompanies.slice(6, 12)"
                :key="tk"
              >
                <div class="label-above">{{ tk }}</div>
                <v-text-field
                  v-model="form[tk.toLowerCase().replace(/[\.\s]/g, '')]"
                  placeholder="100,00 ₽"
                  variant="outlined"
                  dense
                  hide-details
                  class="custom-input"
                ></v-text-field>
              </v-col>
            </v-row>
          </v-col>
        </v-row>
      </v-card-text>
      <v-card-actions class="modal-actions">
        <v-row justify="center" no-gutters>
          <v-col cols="auto">
            <v-btn class="custom-btn cancel-btn" @click="closeModal">Отменить</v-btn>
          </v-col>
          <v-col cols="auto">
            <v-btn class="custom-btn edit-btn" @click="closeModal">Редактировать</v-btn>
          </v-col>
          <v-col cols="auto">
            <v-btn class="custom-btn save-btn" @click="saveModal">Сохранить</v-btn>
          </v-col>
        </v-row>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: "ModalCourier",
  props: {
    item: {
      type: Object,
      default: null,
    },
  },
  data() {
    return {
      dialog: false,
      form: {
        fio: "",
        phone: "",
        abdjarim: "",
        passportType: "",
        passportNumber: "",
        birthDate: "",
        packagesSutki: "",
        packagesNedelia: "",
        packagesMesiac: "",
        moneySutki: "",
        moneyNedelia: "",
        moneyMesiac: "",
        pochtaRossii: "",
        sdek: "",
        boxberry: "",
        kit: "",
        yandex: "",
        delLiniya: "",
        pek: "",
        avito: "",
        gts: "",
        gtsLnrDnr: "",
        pterocha: "",
        wb: "",
        extraField: "", // Дополнительное поле
      },
      transportCompanies: [
        "Почта России",
        "СДЭК",
        "Boxberry",
        "Кит",
        "Яндекс",
        "Дел. Линии",
        "ПЭК",
        "Авито",
        "GTS",
        "GTS ЛНР ДНР",
        "Птеро.ка",
        "WB",
      ],
      passportOptions: ["Дополнительное поле", "Другой документ"],
    };
  },
  watch: {
    item(newItem) {
      if (newItem) {
        this.form = {
          fio: newItem.fio || "",
          phone: newItem.phone || "",
          abdjarim: newItem.abdjarim || "",
          passportType: newItem.passportType || "",
          passportNumber: newItem.passportNumber || "",
          birthDate: newItem.birthDate ? this.formatDate(newItem.birthDate) : "",
          packagesSutki: newItem.packagesSutki || "300 шт.",
          packagesNedelia: newItem.packagesNedelia || "2000 шт.",
          packagesMesiac: newItem.packagesMesiac || "5000 шт.",
          moneySutki: newItem.moneySutki || "20 000,00 ₽",
          moneyNedelia: newItem.moneyNedelia || "150 000,00 ₽",
          moneyMesiac: newItem.moneyMesiac || "600 000,00 ₽",
          pochtaRossii: newItem.pochtaRossii || "100,00 ₽",
          sdek: newItem.sdek || "100,00 ₽",
          boxberry: newItem.boxberry || "100,00 ₽",
          kit: newItem.kit || "100,00 ₽",
          yandex: newItem.yandex || "100,00 ₽",
          delLiniya: newItem.delLiniya || "100,00 ₽",
          pek: newItem.pek || "100,00 ₽",
          avito: newItem.avito || "100,00 ₽",
          gts: newItem.gts || "100,00 ₽",
          gtsLnrDnr: newItem.gtsLnrDnr || "100,00 ₽",
          pterocha: newItem.pterocha || "100,00 ₽",
          wb: newItem.wb || "100,00 ₽",
          extraField: newItem.extraField || "",
        };
      }
    },
  },
  methods: {
    openModal() {
      console.log("Modal opened");
      this.dialog = true;
    },
    closeModal() {
      console.log("Modal closed");
      this.dialog = false;
    },
    saveModal() {
      console.log("Saving form data:", this.form);
      this.$emit("save", this.form);
      this.dialog = false;
    },
    openDatePicker() {
      console.log("Open date picker");
    },
    formatDate(date) {
      if (!date) return "";
      const [year, month, day] = date.split("-");
      return `${day.padStart(2, "0")}.${month.padStart(2, "0")}.${year}`;
    },
  },
};
</script>

<style scoped>
.modal-card {
  border-radius: 5px;
  padding: 16px;
}

.modal-title {
  font-size: 16px;
  font-weight: 300;
  color: rgba(0, 0, 0, 1);
  padding-bottom: 16px;
}

.modal-section {
  margin-bottom: 16px;
}

.section-title {
  font-size: 10px;
  color: rgba(0, 0, 0, 1);
  margin-bottom: 16px;
  font-weight: 300;
}

.label-above {
  font-size: 12px;
  color: #333;
  margin-bottom: 4px;
  text-align: center;
}

/* Стили для редактируемых полей ввода */
:deep(.custom-input .v-field) {
  border: 1px solid #01a7e3 !important;
  border-radius: 5px !important;
  box-shadow: none !important;
  background: transparent !important;
  min-height: 36px !important;
  padding: 0 12px !important;
  font-size: 14px !important;
  display: flex !important;
  align-items: center !important;
}

:deep(.custom-input .v-field__outline) {
  display: none !important;
}

:deep(.custom-input .v-label) {
  font-size: 12px;
  color: #333;
  top: -8px !important;
}

:deep(.custom-input .v-field__input) {
  font-size: 14px !important;
  color: #333 !important;
  line-height: 36px !important;
  padding: 0 !important;
}

/* Стили для выпадающего списка */
:deep(.custom-input .v-select__selection) {
  font-size: 14px;
  color: #333;
  line-height: 36px !important;
}

:deep(.custom-input .v-field__append-inner) {
  align-self: center !important;
  padding-right: 4px !important;
}

:deep(.custom-input .v-field__append-inner .v-icon) {
  font-size: 16px !important;
  color: #01a7e3 !important;
  line-height: 36px !important;
}

/* Стили для статичных полей ("Сутки", "Неделя", "Месяц") */
:deep(.static-input .v-field) {
  border: 1px solid #01a7e3 !important;
  border-radius: 5px !important;
  box-shadow: none !important;
  height: 36px !important; /* Установлена фиксированная высота, чтобы совпадала с custom-input */
  padding: 0 8px !important; /* Уменьшен padding для компактности */
  font-size: 14px !important;
  display: flex !important;
  align-items: center !important;
}

:deep(.static-input .v-field__outline) {
  display: none !important;
}

:deep(.static-input .v-field__input) {
  font-size: 14px !important;
  color: #333 !important;
  line-height: 36px !important;
  padding: 0 !important;
  text-align: center !important; /* Центрирование текста */
}

.modal-actions {
  padding: 16px;
}

/* Стили для кнопок */
.custom-btn {
  min-width: 210px !important;
  height: 36px !important;
  text-transform: none !important;
  margin-left: 3vh;
}

.cancel-btn {
  background-color: #ff0000 !important;
  color: white !important;
  border-radius: 5px !important;
}

.edit-btn {
  background-color: #808080 !important;
  color: white !important;
  border-radius: 5px !important;
}

.save-btn {
  background-color: #01a7e3 !important;
  color: white !important;
  border-radius: 5px !important;
}

/* Стили для секции "Ограничение на кол-во посылок" */
.package-row {
  display: flex;
  flex-wrap: nowrap;
}

.package-gap {
  margin-right: 2vh; /* Отступ между парами инпутов */
}

/* Адаптивность для больших экранов (1920px и выше) */
@media screen and (min-width: 1920px) {
  .modal-card {
    max-width: 800px !important;
  }
  :deep(.custom-input .v-field) {
    width: 150px !important;
    height: 36px !important;
  }
  :deep(.static-input .v-field) {
    width: 30px !important; /* Уменьшена ширина для статичных полей до 50px */
    height: 36px !important;
  }
  .v-col-sm-2 {
    flex: 0 0 16.666667% !important;
    max-width: 16.666667% !important;
  }
  .package-gap {
    margin-right: 2vh !important;
  }
}

/* Адаптивность для средних экранов (960px - 1919px) */
@media screen and (min-width: 960px) and (max-width: 1919px) {
  .modal-card {
    max-width: 600px !important;
  }
  :deep(.custom-input .v-field) {
    width: 120px !important;
    height: 24px !important;
    padding: 0 8px !important;
    font-size: 12px !important;
    line-height: 24px !important;
  }
  :deep(.static-input .v-field) {
    width: 30px !important; /* Уменьшена ширина для статичных полей до 40px */
    height: 24px !important;
    padding: 0 4px !important;
    font-size: 12px !important;
    line-height: 24px !important;
  }
  :deep(.static-input .v-field__input) {
    font-size: 12px !important;
    line-height: 24px !important;
  }
  .v-col-sm-2 {
    flex: 0 0 20% !important;
    max-width: 20% !important;
  }
  .package-gap {
    margin-right: 1.5vh !important;
  }
}

/* Адаптивность для меньших экранов (меньше 960px) */
@media screen and (max-width: 959px) {
  .modal-card {
    max-width: 100% !important;
    width: 100% !important;
  }
  .modal-section {
    flex-direction: column;
  }
  .v-col-sm-6,
  .v-col-sm-4,
  .v-col-sm-2 {
    flex: 0 0 100% !important;
    max-width: 100% !important;
    margin-bottom: 8px !important;
  }
  :deep(.custom-input .v-field) {
    width: 100% !important;
    height: 36px !important;
    padding: 0 12px !important;
    font-size: 14px !important;
    line-height: 36px !important;
  }
  :deep(.static-input .v-field) {
    width: 100% !important;
    height: 36px !important;
    padding: 0 12px !important;
    font-size: 14px !important;
    line-height: 36px !important;
  }
  :deep(.static-input .v-field__input) {
    font-size: 14px !important;
    line-height: 36px !important;
  }
  .package-gap {
    margin-right: 0 !important;
  }
}
</style>
