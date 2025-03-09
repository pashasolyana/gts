<template>
  <v-dialog v-model="dialog" max-width="400">
    <v-card class="modal-card">
      <v-card-title class="modal-title">Успешно</v-card-title>
      <v-card-text class="modal-text">
        <div class="recipient-name">Абдукарим Гахраманович И.</div>
        <div class="transfer-amount">400,00 ₽</div>
      </v-card-text>
    </v-card>
  </v-dialog>
</template>

<script setup lang="ts">
import { ref } from "vue";

const dialog = ref(true);
</script>

<style scoped>
.modal-card {
  border: 1px solid rgba(1, 167, 227, 1); /* Цвет рамки */
  border-radius: 10px !important; /* Скругление углов */
  padding: 20px; /* Отступы внутри карточки */
}

.modal-title {
  font-size: 18px; /* Размер текста заголовка */
  text-align: center; /* Центрирование заголовка */
  color: rgba(0, 0, 0, 0.5);
}

.modal-text {
  text-align: center; /* Центрирование текста */
  color: rgba(0, 0, 0, 0.5);
  font-size: 13px;
}

.recipient-name {
  font-size: 13px;
  color: rgba(0, 0, 0, 0.5);
  margin-bottom: 10px; /* Отступ снизу */
}

.transfer-amount {
  font-size: 20px; /* Размер текста суммы */
  color: rgba(0, 193, 74, 1); /* Цвет суммы */
}
</style>
