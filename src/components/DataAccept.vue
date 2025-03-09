<template>
  <v-dialog v-model="dialog" max-width="400">
    <v-card class="modal-card">
      <v-card-title class="modal-title">Подтвердите данные</v-card-title>
      <v-card-text class="modal-text">
        <div class="title">Получатель</div>
        <div class="text">Абдукарим Гахраманович И.</div>
        <div class="title">Банк получателя</div>
        <div class="text">Альфа-банк</div>
        <div class="title">Номер телефона</div>
        <div class="text">+7 (966) 333-33-68</div>
        <div class="title">Сумма перевода</div>
        <div class="amount">400,00 ₽</div>
      </v-card-text>
      <v-card-actions>
        <v-btn class="confirm-button" @click="transfer">Перевести</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script setup lang="ts">
import { ref } from "vue";

const dialog = ref(true);
function transfer() {
  dialog.value = false;
}
</script>

<style scoped>
.modal-card {
  border-radius: 16px !important; /* Скругление углов */
  padding: 20px; /* Отступы внутри карточки */
}

.modal-title {
  font-size: 16px; /* Размер текста заголовка */
  text-align: center; /* Центрирование заголовка */
}

.title {
  color: rgba(0, 0, 0, 0.5);
  font-size: 12px;
  margin-top: 1vh;
}

.text {
  color: rgba(0, 0, 0, 1);
  font-size: 14px;
}

.amount {
  color: rgba(0, 193, 74, 1);
  font-size: 20px;
}

.modal-text {
  font-size: 12px; /* Размер текста */
  margin-bottom: 20px; /* Отступ снизу */
  text-align: left; /* Выравнивание текста влево */
}

.confirm-button {
  background-color: #00c144; /* Зеленый цвет кнопки */
  color: white; /* Цвет текста на кнопке */
  width: 100%; /* Ширина кнопки */
  border-radius: 10px; /* Скругление углов кнопки */
}
</style>
