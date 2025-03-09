<template>
  <v-dialog v-model="dialog" max-width="500">
    <v-card class="modal-card">
      <v-card-title></v-card-title>
      <v-card-text class="modal-title"> Внимание! </v-card-text>
      <v-card-text class="modal-text">
        Вы хотите вывести средства, для этого заполните паспортные данные
      </v-card-text>
      <v-card-actions class="modal-actions">
        <v-btn class="cancel-button" @click="cancel">Отменить</v-btn>
        <v-btn class="confirm-button">Заполнить</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script lang="ts" setup>
import { ref } from "vue";

const dialog = ref(true);

function cancel() {
  dialog.value = false;
}
</script>

<style>
.modal-card {
  border: 2px solid rgba(1, 167, 227, 1); /* Цвет рамки */
  border-radius: 24px !important; /* Скругление углов с !important */
}

.modal-title {
  text-align: center;
  font-weight: 500;
  font-size: 13px;
  color: rgba(0, 0, 0, 0.5);
}

.modal-text {
  text-align: center;
  font-size: 13px;
  margin-top: -4vh;
  color: rgba(0, 0, 0, 0.5);
}

.modal-actions {
  justify-content: center;
}

.cancel-button {
  background-color: rgba(252, 20, 20, 1);
  color: white;
  width: 120px;
  margin-right: 10px;
  font-size: 10px;
}

.confirm-button {
  background-color: rgba(1, 167, 227, 1); /* Цвет кнопки "Заполнить" */
  color: white;
  width: 120px;
  font-size: 10px;
}
</style>
