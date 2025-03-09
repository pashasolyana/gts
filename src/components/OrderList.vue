<template>
  <div class="container">
    <div class="content-wrapper">
      <div class="card">
        <!-- Header: Logo and User Info -->
        <div class="header">
          <div class="logo">
            <img
              src="../assets/logo.png"
              style="max-height: 69px; max-width: 173px"
              alt="AGTS Logo"
            />
          </div>
          <div class="user-info">
            <div class="user-details">
              <div class="user-surname">Исмоилов</div>
              <div class="user-name">Абдукарим</div>
              <div class="user-phone">+7 (888) 999 10 10</div>
            </div>
            <div class="user-actions">
              <img src="../assets/empty-user.png" class="user-avatar" alt="User Avatar" />
              <img src="../assets/settings.png" class="settings-icon" alt="Settings" />
            </div>
          </div>
        </div>

        <!-- Balance Card -->
        <div class="balance-card">
          <!-- Payout (Green) -->
          <div class="balance-line__left">
            <div class="balance-line__item">
              <span class="balance-label">Выплатить выбранное</span>
            </div>
            <div class="balance-line__item">
              <span class="balance-label">Положить на депозит выбранное</span>
            </div>
          </div>

          <!-- Deposit (Blue with Bonus) -->
          <div class="balance-line__right">
            <div>
              <div class="balance-row__first">
                <div class="balance-row__green">
                  <span class="balance-value">400,00 ₽</span>
                  <span class="balance-action" @click="openModal('payout')"
                    >Выплатить</span
                  >
                </div>
              </div>
              <div class="balance-row__second">
                <div class="balance-row__blue">
                  <span class="balance-value">400,00 ₽</span>
                  <span class="balance-action" @click="openModal('deposit')"
                    >Пополнить</span
                  >
                </div>
              </div>

              <div class="bonus">
                <span class="bonus-text"
                  >Через 30 дней можно вывести <span class="procent">+5%</span></span
                >
                <span class="bonus-amount">420,00 ₽</span>
              </div>
            </div>
          </div>
        </div>

        <div class="balance-card mt-2">
          <!-- Pending (Gray) -->
          <div class="balance-line__left_second">
            <div class="balance-line__item">
              <span class="balance-label">Ожидаются</span>
            </div>
            <div class="balance-line__item">
              <span class="balance-label">Заработано</span>
            </div>
          </div>

          <!-- Earned (Green) -->
          <div class="balance-line__right">
            <div class="balance-row__first">
              <div class="balance-row__gray">
                <span class="balance-value">300 ₽</span>
                <span class="balance-action">4 шт.</span>
              </div>
            </div>
            <div class="balance-row__second">
              <div class="balance-row__green_second">
                <span class="balance-value">200 ₽</span>
                <span class="balance-action">2 шт.</span>
              </div>
            </div>
          </div>
        </div>

        <div class="buttons mb-4 mt-4">
          <button class="button orange">Все (7)</button>
          <button class="button blue">К выплате (4)</button>
          <button class="button blue">Создан (3)</button>
          <button class="button blue">Выплачено (2)</button>
        </div>
      </div>

      <div class="transaction-list">
        <div
          v-for="(transaction, index) in paginatedTransactions"
          :key="transaction.id"
          class="card-content mt-2"
          :class="['card', { active: selectedIndices.includes(index) }]"
          @click="toggleSelection(index)"
          :style="{
            background: getBackgroundColor(transaction),
          }"
        >
          <div class="info-container">
            <div class="top-row">
              <div class="date-status">
                {{ transaction.date }}
              </div>
              <div
                class="status"
                :style="{
                  color: selectedIndices.includes(index)
                    ? 'white'
                    : getAmountTaskColor(transaction),
                }"
              >
                {{ transaction.status }}
              </div>
            </div>
            <div
              class="number"
              :style="{
                color: selectedIndices.includes(index)
                  ? 'white' // Цвет текста при выборе карточки
                  : transaction.action == 'На депозите'
                  ? 'rgba(255, 255, 255, 1)'
                  : transaction.action == 'Выдан'
                  ? 'rgba(255, 255, 255, 1)'
                  : 'rgba(0, 0, 0, 0.2)',
              }"
            >
              {{ transaction.id }}
            </div>
          </div>
          <div>
            <div
              class="amount"
              :style="{
                color: selectedIndices.includes(index)
                  ? 'white'
                  : getAmountTaskColor(transaction),
              }"
            >
              {{ transaction.amount }} ₽
            </div>
          </div>
          <div class="right-side">
            <div class="action-button" @click.stop="sendToCard('sended')">
              {{ transaction.action }}
            </div>
          </div>
        </div>
      </div>
      <Withdraw
        v-model="isModalOpen"
        :title="modalTitle"
        amount="400"
        :type="modalType"
        :message="modalMessage"
        @confirm="confirmAction"
        @cancel="closeModal"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";
import Withdraw from "./Withdraw.vue";

const activeIndex = ref(null);

// Reactive array of transactions
const transactions = ref([
  {
    id: "880099702098181",
    date: "14.02.2024",
    status: "Создан",
    amount: "100",
    action: "На карту",
  },
  {
    id: "880099702098182",
    date: "14.02.2024",
    status: "Принят",
    amount: "100",
    action: "На карту",
  },
  {
    id: "880099702098183",
    date: "14.02.2024",
    status: "Принят",
    amount: "100",
    action: "На карту",
  },
  {
    id: "880099702098184",
    date: "14.02.2024",
    status: "Создан",
    amount: "140",
    action: "На карту",
  },
  {
    id: "880099702098185",
    date: "14.02.2024",
    status: "Принят",
    amount: "120",
    action: "На карту",
  },
  {
    id: "880099702098186",
    date: "14.02.2024",
    status: "Выдан",
    amount: "100",
    action: "На депозите",
  },
  {
    id: "880099702098187",
    date: "14.02.2024",
    status: "Выдан",
    amount: "100",
    action: "Выдан",
  },
  {
    id: "880099702098188",
    date: "14.02.2024",
    status: "Выдан",
    amount: "100",
    action: "На депозите",
  },
]);

const getColor = (transaction: { status: string }) => {
  switch (transaction.status) {
    case "Создан":
      return "rgba(0, 0, 0, 0.8)"; // Gray for "Создан"
    case "Принят":
      return "rgba(0, 193, 74, 1)"; // Green for "Принят"
    case "Поступил":
      return "rgba(0, 193, 74, 1)"; // Green for "Поступил"
    case "Выплачен":
      return "rgba(1, 167, 227, 1)"; // Blue for "Выплачен"
    case "Выдан":
      return "rgba(255, 255, 255, 1)"; // Green for "Выдан"
    case "На депозите":
      return "rgba(255, 255, 255, 1)"; // Dark blue for "Выдан на депозит"
    default:
      return "rgba(0, 0, 0, 0.5)"; // Default gray
  }
};

const getAmountTaskColor = (transaction: { status: string }) => {
  // You can define your own logic for amount color based on status
  switch (transaction.status) {
    case "Создан":
      return "rgba(205, 205, 205, 1)"; // Light gray for "Создан"
    case "Принят":
      return "rgba(0, 193, 74, 1)"; // Green for "Принят"
    case "Поступил":
      return "rgba(0, 193, 74, 1)"; // Green for "Поступил"
    case "Выдан":
      return "rgba(255, 255, 255, 1)"; // Blue for "Выплачен"
    case "Выдан на депозит":
      return "rgba(1, 87, 155, 0.9)"; // Dark blue for "Выдан на депозит"
    default:
      return "rgba(205, 205, 205, 1)"; // Default light gray
  }
};

const getBackgroundColor = (transaction: { status: string }) => {
  if (transaction.status === "Создан") {
    return "linear-gradient(to right, #ffffff 70%, rgba(176, 190, 197, 0.9) 60%)"; // White left, gray right for "Создан"
  } else if (transaction.status === "Принят") {
    return "linear-gradient(to right, #ffffff 70%, rgba(0, 193, 74, 1) 60%)"; // White left, green right for "Принят"
  } else if (transaction.status === "Поступил") {
    return "linear-gradient(to right, #ffffff 70%, rgba(0, 193, 74, 0.9) 60%)"; // White left, green right for "Поступил"
  } else if (transaction.status === "Выплачен") {
    return "linear-gradient(to right, #ffffff 70%, rgba(1, 167, 227, 0.9) 60%)"; // White left, blue right for "Выплачен"
  } else if (transaction.status === "Выдан") {
    if (transaction.action == "На депозите") {
      return "linear-gradient(to right, #ffffff 0%, rgba(1, 167, 227, 1) 0%)";
    } else if (transaction.action == "Выдан") {
      return "linear-gradient(to right, #ffffff 0%, rgba(0, 193, 74, 0.5) 0%)";
    }
    return "linear-gradient(to right, #ffffff 70%, rgba(0, 193, 74, 0.9) 60%)"; // White left, green right for "Выдан"
  }
};

// Pagination variables
const itemsPerPage = ref(10); // Number of items per page
const currentPage = ref(1); // Current page number

// Computed property for paginated transactions
const paginatedTransactions = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage.value;
  const end = start + itemsPerPage.value;
  return transactions.value.slice(start, end);
});

// Computed property for total pages
const totalPages = computed(() => {
  return Math.ceil(transactions.value.length / itemsPerPage.value);
});

// Methods for pagination navigation (optional, already handled by buttons)
const goToPage = (page: number) => {
  currentPage.value = page;
};

const isModalOpen = ref(false);
const modalType = ref("");
const modalTitle = ref("");
const modalMessage = ref("");

const openModal = (type: "payout" | "deposit") => {
  if (type === "payout") {
    modalTitle.value = "Подтверждение выплаты";
    modalMessage.value = "Вы уверены, что хотите выплатить 400,00 ₽?";
    modalType.value = "payout";
  } else if (type === "deposit") {
    modalTitle.value = "Подтверждение пополнения";
    modalMessage.value = "Вы уверены, что хотите положить 400,00 ₽ на депозит?";
    modalType.value = "deposit";
  }
  isModalOpen.value = true;
};

const closeModal = () => {
  isModalOpen.value = false;
  modalTitle.value = "";
  modalMessage.value = "";
};

const confirmAction = () => {
  if (modalTitle.value.includes("выплаты")) {
    console.log("Payout confirmed: 400,00 ₽");
  } else if (modalTitle.value.includes("пополнения")) {
    console.log("Deposit confirmed: 400,00 ₽");
  }
  closeModal();
};

const sendToCard = (message) => {
  alert(message);
};

const selectedIndices = ref([]); // Массив для хранения выбранных индексов

const toggleSelection = (index) => {
  const transaction = transactions.value[index];
  if (transaction.status !== "Принят") return; // Разрешаем выбор только для статуса "Принят"

  if (selectedIndices.value.includes(index)) {
    // Если индекс уже выбран, удаляем его
    selectedIndices.value = selectedIndices.value.filter((i) => i !== index);
  } else {
    // Если индекс не выбран, добавляем его
    selectedIndices.value.push(index);
  }
};
</script>
<style lang="css" scoped>
.container {
  max-width: 375px;
  max-height: 1142px;
  margin: 0 auto; /* Центрирование */
  overflow: hidden; /* Исключение скролла */
}

.content-wrapper {
  padding: 34px 6px 14px 8px;
}

.header {
  height: max-content;
  display: flex;
  justify-content: space-between;
}

.user-info {
  gap: 9px;
  display: flex;
  align-items: center;
  margin-right: 13px;
}

.user-details {
  text-align: right;
  font-size: 10px;
}

.user-name {
  color: #00000080;
}

.user-phone {
  font-size: 8px;
  color: #00000080;
}

.user-actions {
  display: flex;
  align-items: center;
  gap: 16px;
}

.balance-card {
  display: flex;
  align-items: center;
  gap: 9px;
  border-radius: 20px;
  box-shadow: 0px 4px 4px 0px rgba(0, 0, 0, 0.25);
  background: rgb(255, 255, 255);
  height: 108px;
}

.balance-line__left {
  margin: 0px 0px 0px 12px;
}

.balance-line__left_second {
  display: flex;
  flex-direction: column;
  gap: 15px;
  margin: 0px 0px 0px 12px;
}

.balance-line__item {
  width: 73px;
  font-size: 13px;
  color: #00000080;
  line-height: 123%;
}

.balance-line__item:nth-child(2) {
  margin-top: 8px;
}

.balance-line__right {
  margin-right: 8px;
  width: 100%;
}

.balance-row__green {
  background-color: #00c14a;
  display: flex;
  justify-content: space-between;
  font-size: 13px;
  color: #fff;
  border-radius: 10px;
}

.balance-row__green_second {
  background-color: #80e1a5;
  display: flex;
  justify-content: space-between;
  font-size: 13px;
  color: #fff;
  border-radius: 10px;
}

.balance-row__gray {
  background-color: #c8c8c8;
  display: flex;
  justify-content: space-between;
  font-size: 13px;
  color: #fff;
  border-radius: 10px;
}

.balance-value {
  margin: 5px 0px 5px 12px;
}

.balance-action {
  margin: 5px 12px 5px 0px;
}

.bonus {
  height: max-content;
  margin-left: 6px;
}

.bonus-text {
  font-size: 9px;
}

.procent {
  color: #00c14a;
  margin-left: 8px;
  font-size: 13px;
}
.bonus-amount {
  color: #01a7e3;
  margin-left: 28px;
  font-size: 13px;
}

.balance-row__blue {
  background-color: #01a7e3;
  display: flex;
  justify-content: space-between;
  font-size: 13px;
  color: #fff;
  border-radius: 10px;
}

.balance-row__first {
  margin-top: 9px;
}

.balance-row__second {
  margin-top: 9px;
}

.button {
  color: #fff;
  border-radius: 10px;
  padding: 6px 12px;
  font-size: 11px;
}

.buttons {
  display: flex;
  justify-content: space-between;
  margin: 0px 12px;
}

.orange {
  background-color: #f8802a;
}

.blue {
  background-color: #01a7e3;
}

.transaction-list {
  margin-bottom: 20px;
  margin-top: 20px;
}

.card-content {
  border-radius: 20px;
  box-shadow: 0px 4px 4px 0px rgba(0, 0, 0, 0.25);
  background: rgb(255, 255, 255);
}

.date-status,
.status {
  font-size: 10px;
}

.number {
  font-size: 13px;
}

.amount {
  font-size: 24px;
}

.action-button {
  color: #fff;
}

.card-content {
  display: grid;
  grid-template-columns: 1.5fr 0.9fr 1fr;
  grid-template-rows: auto auto;
  grid-auto-flow: row;
  margin-left: 12px;
  margin-right: 2px;
  padding: 7px 0px;
}

.top-row {
  display: flex;
  gap: 26px;
}

.info-container {
  margin-left: 15px;
}

.right-side {
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 13px;
}

.card {
  transition: background 0.3s;
  cursor: pointer;
}

.card.active {
  /* background: #00c14a !important; */
  background: linear-gradient(
    to right,
    #00c14a 0%,
    rgba(0, 193, 74, 0.7) 100%
  ) !important;
  color: white !important;
}
@media (max-width: 375px) {
  .bonus {
    display: flex;
    gap: 3vw;
  }

  .bonus-text {
    font-size: 2.5vw;
  }

  .procent {
    font-size: 3vw;
  }

  .bonus-amount {
    margin-left: 0;
    font-size: 3.5vw;
  }

  .buttons {
    margin: 0px 4px;
  }

  .button {
    font-size: 2.5vw;
  }

  .user-actions {
    gap: 2vw;
  }

  .user-details {
    font-size: 8px;
  }
}
</style>
