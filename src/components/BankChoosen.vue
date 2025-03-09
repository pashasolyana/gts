<template>
  <v-dialog v-model="dialog" max-width="420">
    <v-card class="pa-4 rounded-lg">
      <!-- Заголовок и логотип в одну строку -->
      <div class="title-container">
        <div class="text">Выберите банк</div>
        <v-img src="../assets/sbp.svg" max-height="24" class="mr-14" contain />
      </div>

      <!-- Поле поиска (обычный input) -->
      <div class="search-container">
        <input
          v-model="search"
          type="text"
          placeholder="Поиск банка"
          class="search-input"
        />
        <v-icon class="search-icon">mdi-magnify</v-icon>
      </div>

      <!-- Заголовок для популярных банков -->
      <div class="popular-title">Популярные банки</div>

      <!-- Список банков -->
      <v-list class="bank-list">
        <v-list-item
          v-for="bank in filteredBanks"
          :key="bank.id"
          @click="selectBank(bank)"
          class="balance-card"
          :class="{ 'selected-bank': selectedBank?.id === bank.id }"
        >
          <template v-slot:prepend>
            <v-avatar size="40">
              <v-img src="../assets/alfa.svg" />
            </v-avatar>
          </template>
          <v-list-item-title class="bank-name">
            {{ bank.name }}
          </v-list-item-title>
        </v-list-item>
      </v-list>

      <!-- Кнопка "Продолжить" -->
      <v-card-actions class="justify-center mt-4">
        <v-btn
          variant="elevated"
          class="text-white rounded-pill continue-btn"
          width="90%"
          height="31"
          :disabled="!selectedBank"
          @click="withdraw"
        >
          Продолжить
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";

const dialog = ref(true);
const selectedBank = ref(null);
const search = ref("");

const banks = ref([
  { id: 1, name: "Альфа-банк", logo: "../assets/alfa.svg" },
  { id: 2, name: "Т-Банк", logo: "../assets/t-bank.svg" },
  { id: 3, name: "Сбер", logo: "../assets/sber.svg" },
  { id: 4, name: "ВТБ", logo: "../assets/vtb.svg" },
  { id: 5, name: "Совком Банк", logo: "../assets/sovcom.svg" },
]);

const filteredBanks = computed(() =>
  banks.value.filter((b) => b.name.toLowerCase().includes(search.value.toLowerCase()))
);

function selectBank(bank) {
  selectedBank.value = bank;
}

function withdraw() {
  dialog.value = false;
}
</script>

<style scoped>
/* Заголовок и логотип */
.title-container {
  display: flex;
  align-items: center;
  font-size: 13px;
  font-weight: 500;
  margin-left: 10vh;
  margin-bottom: 1.5vh;
  color: rgba(0, 0, 0, 1);
}

.title-container v-img {
  display: flex;
  margin-left: -1vh;
}

/* Поле поиска */
.search-container {
  position: relative;
  display: flex;
  align-items: center;
  width: 100%;
  background: white;
  border-radius: 20px;
  border: 1px solid rgba(1, 167, 227, 1);
  padding: 4px 12px;
}

.search-input {
  flex-grow: 1;
  border: none;
  outline: none;
  padding: 6px;
  font-size: 14px;
  border-radius: 20px;
}

.search-icon {
  font-size: 18px;
  color: rgba(0, 0, 0, 0.5);
}

/* Заголовок популярных банков */
.popular-title {
  font-size: 15px;
  font-weight: 500;
  color: rgba(0, 0, 0, 0.5);
  margin-top: 12px;
  margin-bottom: 2px;
  text-align: center;
}

/* Карточка банка */
.balance-card {
  border-radius: 20px !important;
  padding: 8px;
  background-color: white;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
  display: flex;
  align-items: center;
  cursor: pointer;
  margin-bottom: 1vh;
  color: rgba(0, 0, 0, 0.5);
}

.balance-card:hover {
  background: #f0f0f0;
}

.selected-bank {
  border: 1px solid rgba(0, 193, 74, 1);
  background: white;
}

/* Название банка */
.bank-name {
  font-size: 16px;
  font-weight: 500;
}

/* Кнопка продолжить */
.continue-btn {
  letter-spacing: 1px;
  font-size: 10px;
  border-radius: 10px !important;
  background-color: rgba(0, 193, 74, 1);
}
</style>
