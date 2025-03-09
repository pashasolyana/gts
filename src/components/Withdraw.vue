<template>
  <div
    class="modal"
    v-if="modelValue"
    aria-hidden="!modelValue"
    aria-label="Modal"
    role="dialog"
    @click="handleOutsideClick"
  >
    <div class="modal-content" ref="modalContent">
      <div class="modal-body">
        <div v-if="type === 'payout'" class="payout-content">
          <div class="card-content mt-2">
            <div class="info-container">
              <div class="number">Сумму которую хотите вывести?</div>
            </div>
            <div class="right-side">
              <div class="action-button">400, 00 ₽</div>
            </div>
          </div>
          <div class="card-content-phone mt-2">
            <div class="info-container">
              <div class="number">Введите номер телефона</div>
            </div>
            <img src="../assets/sbp.svg" class="image-icon" />
            <div class="right-side">
              <div class="action-button-phone">Вывести</div>
            </div>
          </div>
        </div>
        <div v-if="type == 'deposit'" class="payout-content">
          <div class="card-content mt-2">
            <div class="info-container"></div>
            <div class="right-side">
              <div class="action-button-blue">400, 00 ₽</div>
            </div>
          </div>
          <div class="text">Введите сумму, которую хотите положить на депозит</div>
          <div class="withdraw">
            <div @click.stop="toggleDropdown" class="modal-select">
              <div
                :style="{
                  marginTop: '.3vh',
                }"
              >
                {{ selectedOption || "Выберите срок" }}
              </div>
              <div v-if="isDropdownOpen" class="dropdown-options">
                <div
                  v-for="option in depositOptions"
                  :key="option.days"
                  class="dropdown-option"
                  @click.stop="selectOption(option)"
                >
                  {{ option.label }} <span class="select-amount">{{ option.total }}</span
                  ><span class="bonus">+ {{ option.plus }}</span>
                </div>
              </div>
            </div>
            <div>
              <button class="withdraw-btn">Положить</button>
            </div>
          </div>
          <div>
            <span class="select-amount">420, 00 ₽</span
            ><span class="bonus-withdraw">+ 20, 00 ₽</span>
          </div>
        </div>
      </div>
    </div>
    <!-- <DocumentRequired /> -->
    <!-- <BigAmount /> -->
    <!-- <BankChoosen /> -->
    <!-- <DataAccept /> -->
    <WithdrawSuccess />
  </div>
</template>

<script setup lang="ts">
import { ref, onUnmounted, onMounted } from "vue";
import DocumentRequired from "./DocumentRequired.vue";
import BigAmount from "./BigAmount.vue";
import BankChoosen from "./BankChoosen.vue";
import DataAccept from "./DataAccept.vue";
import WithdrawSuccess from "./WithdrawSuccess.vue";

const props = defineProps<{
  modelValue: boolean;
  type: "payout" | "deposit";
  amount: string;
}>();
const emit = defineEmits<{
  (e: "update:modelValue", value: boolean): void;
  (e: "confirm"): void;
  (e: "cancel"): void;
  (e: "phone"): void;
}>();

const isDropdownOpen = ref(false);
const selectedBonus = ref<string>("");
const modalContent = ref<HTMLElement | null>(null);
const displayAmount = ref<string>(props.amount || "400,00 ₽");
const selectedOption = ref<string>("");

const depositOptions = ref([
  { days: 7, label: "Через 7 дней", total: "206 000,00 ₽", plus: "1 000,00 ₽" },
  { days: 14, label: "Через 14 дней", total: "207 000,00 ₽", plus: "2 000,00 ₽" },
  { days: 21, label: "Через 21 день", total: "208 00,00 ₽", plus: "3 000,00 ₽" },
  { days: 30, label: "Через 30 дней", total: "210 000,00 ₽", plus: "5 000,00 ₽" },
]);

const toggleDropdown = () => {
  isDropdownOpen.value = !isDropdownOpen.value;
};

const handleOutsideClick = (event: MouseEvent) => {
  if (modalContent.value && !modalContent.value.contains(event.target as Node)) {
    closeModal();
  }
};

const closeModal = () => {
  emit("update:modelValue", false);
  isDropdownOpen.value = false;
};

const selectBonus = (bonus: string) => {
  selectedBonus.value = bonus;
  isDropdownOpen.value = false;
};

const selectOption = (option: { label: string }) => {
  selectedOption.value = option.label;
  isDropdownOpen.value = false;
};

const confirm = () => {
  emit("confirm", selectedBonus.value);
  emit("update:modelValue", false);
};

onMounted(() => {
  // Убрали closeDropdownOutside, так как он больше не нужен
});

onUnmounted(() => {
  // Убрали removeEventListener
});
</script>

<style scoped>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
  z-index: 1000;
}

.withdraw {
  display: flex;
}

.withdraw-btn {
  background-color: rgba(1, 167, 227, 1);
  border-radius: 10px;
  padding: 8px;
  font-size: 10px;
  color: white;
  width: 150%;
  height: 32px;
  margin-top: 1.5vh;
}

.image-icon {
  margin-right: 6vh;
  width: 48px; /* Установите начальный размер для изображения */
  height: auto; /* Автоматическая высота, чтобы сохранить пропорции */
}

/* Медиа-запросы для разных разрешений экрана */
@media (max-width: 1024px) {
  .image-icon {
    width: 48px; /* Уменьшите размер для экранов до 1024px */
    margin-right: 4vh; /* Уменьшите отступ справа */
  }
}

@media (max-width: 768px) {
  .image-icon {
    width: 48px; /* Уменьшите размер для мобильных устройств */
    margin-right: 3vh; /* Уменьшите отступ справа */
  }
}

@media (max-width: 600px) {
  .image-icon {
    width: 48px; /* Уменьшите размер для экранов до 600px */
    margin-right: 2.5vh; /* Уменьшите отступ справа */
  }
}

@media (max-width: 480px) {
  .image-icon {
    width: 48px; /* Уменьшите размер для очень маленьких экранов */
    margin-right: 3.2vh; /* Уменьшите отступ справа */
  }
}

@media (max-width: 360px) {
  .image-icon {
    width: 48px; /* Уменьшите размер для самых маленьких экранов */
    margin-right: 1.5vh; /* Уменьшите отступ справа */
  }
}

.modal-content {
  background-color: white;
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
  width: 100%;
  max-width: 450px;
}

.modal-body {
  display: flex;
  flex-direction: column;
  gap: 15px;
  align-items: stretch;
}

.arrow {
  color: rgba(1, 167, 227, 1);
  font-size: 24px;
}

.modal-input {
  position: relative;
  width: 100%;
}

.modal-select {
  border-radius: 10px;
  border: 1px solid #01a7e3;
  color: rgba(0, 0, 0, 0.5);
  height: 32px;
  padding: 5px 5px 5px 15px;
  font-size: 10px;
  width: 65%;
  margin-right: 1vh; /* Space between select and button */
  margin-top: 1.5vh;
  background-image: url('data:image/svg+xml;utf8,<svg fill="%2301A7E3" height="24" viewBox="0 0 24 24" width="24" xmlns="http://www.w3.org/2000/svg"><path d="M7 10l5 5 5-5z"/><path d="M0 0h24v24H0z" fill="none"/></svg>');
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  background-image: url('data:image/svg+xml;utf8,<svg fill="%2301A7E3" height="24" viewBox="0 0 24 24" width="24" xmlns="http://www.w3.org/2000/svg"><path d="M7 10l5 5 5-5z"/><path d="M0 0h24v24H0z" fill="none"/></svg>');
  background-repeat: no-repeat;
  background-position: right 8px center;
  background-size: 32px;
}

.placeholder {
  position: absolute;
  left: 10px;
  top: 12px;
  color: #999;
  font-size: 14px;
  pointer-events: none;
  transition: 0.2s ease all;
}

.text {
  color: rgba(0, 0, 0, 0.5);
  font-size: 11px;
  font-family: "Cabin", sans-serif;
  margin-left: 2vh;
  margin-top: 0.5vh;
}

.modal-input-field {
  border-radius: 10px;
  border: 2px solid #01a7e3;
  height: 48px;
  padding: 10px 10px 10px 10px;
  font-size: 18px;
  width: 100%;
  text-align: right;
  color: #00c14a;
  margin-top: 20px; /* Space for placeholder */
}

.select-option {
  font-size: 14px; /* Размер шрифта для опций */
  color: rgba(0, 0, 0, 0.5); /* Цвет текста для опций */
}

.bonus {
  color: #00c14a; /* Цвет для бонуса */
  margin-left: 0.5vh;
}

.card-content {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px; /* Default padding for desktop */
  height: 40px !important; /* Fixed height to match the mockup */
  border-radius: 15px 15px 15px 15px; /* Rounded corners only on right edges, flat left edge */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Subtle shadow */
  border: 1px solid rgba(1, 167, 227, 1);
}

.select-amount {
  color: rgba(1, 167, 227, 1);
  font-size: 10px;
  margin-left: 1vh;
}

.card-content-phone {
  background: linear-gradient(to right, #ffffff 70%, rgba(0, 193, 74, 0.9) 60%);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px; /* Default padding for desktop */
  height: 40px !important; /* Fixed height to match the mockup */
  border-radius: 15px 15px 15px 15px; /* Rounded corners only on right edges, flat left edge */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Subtle shadow */
  border: 1px solid rgba(1, 167, 227, 1);
}

.info-container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  flex: 1;
  overflow: hidden;
}

.top-row {
  display: flex;
  align-items: center;
  gap: 5px;
  margin-bottom: 2px; /* Space between date/status and ID */
}

.date-status,
.status {
  font-size: 10px; /* Default font size for desktop */
  color: rgba(0, 0, 0, 0.5);
  margin-right: 5px; /* Reduced margin for better spacing */
}

.number {
  font-size: 10px; /* Default font size for desktop */
  color: rgba(0, 0, 0, 0.5);
  margin-top: 1px;
}

/* Action Button */
.action-button {
  border-radius: 20px; /* Match the card-content’s right rounded corners */
  color: rgba(0, 193, 74, 1);
  cursor: pointer;
  font-family: "Cabin", sans-serif;
  height: 30px; /* Match the button height in the mockup */
  display: flex;
  font-size: 15px;
  align-items: left;
  justify-content: start;
  margin-right: 2vh;
  margin-top: 1vh;
}

.action-button-blue {
  border-radius: 20px; /* Match the card-content’s right rounded corners */
  color: rgba(1, 167, 227, 1);
  cursor: pointer;
  font-family: "Cabin", sans-serif;
  height: 30px; /* Match the button height in the mockup */
  display: flex;
  font-size: 15px;
  align-items: left;
  justify-content: start;
  margin-right: 2vh;
  margin-top: 1vh;
}

.action-button-phone {
  border-radius: 20px; /* Match the card-content’s right rounded corners */
  color: white;
  cursor: pointer;
  font-family: "Cabin", sans-serif;
  height: 30px; /* Match the button height in the mockup */
  display: flex;
  font-size: 15px;
  align-items: left;
  justify-content: start;
  margin-right: 1vh;
  margin-top: 1vh;
}

/* Action Button Content */
.action-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
}

.action-text {
  font-size: 10px; /* Default font size for desktop */
  line-height: 1; /* Ensure tight vertical stacking */
}

.action-amount {
  font-size: 10px; /* Default font size for desktop */
  font-weight: bold;
  line-height: 1; /* Ensure tight vertical stacking */
}

.amount {
  color: rgba(205, 205, 205, 1);
  margin-right: 7vh;
  margin-bottom: 1vh;
  font-size: 24px;
}

.modal-phone-container {
  display: flex;
  flex-direction: column;
  width: 100%;
}

.modal-phone-container .modal-input-field.phone {
  border-color: #01a7e3;
  padding-right: 50px; /* Space for icon */
}

.sbp-icon {
  width: 20px;
  height: 20px;
  position: absolute;
  right: 10px;
  top: 14px; /* Center the icon vertically */
}

.modal-action-button {
  border-radius: 10px;
  border: none;
  padding: 10px;
  font-size: 18px;
  cursor: pointer;
  width: 100%;
  color: white;
  background-color: #00c14a;
  margin-top: 10px; /* Space above button */
}

.modal-action-button:hover {
  opacity: 0.9;
}

.modal-dropdown {
  width: 100%;
  position: relative;
}

.selected-option {
  border-radius: 10px;
  border: 2px solid #01a7e3;
  background-color: white;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 10px;
  cursor: pointer;
}

.dropdown-options {
  position: absolute;
  border: 1px solid #01a7e3;
  background-color: white;
  border-radius: 10px;
  width: 70%;
  z-index: 1001;
  margin-left: -2vh;
  margin-top: 1.5vh;
}

.dropdown-option {
  padding: 10px;
  cursor: pointer;
}

.dropdown-option:hover {
  background-color: #f0f4f8;
}

.bonus-withdraw {
  color: #00c14a;
  font-size: 10px;
  margin-left: 1vh;
}

.bonus {
  color: #00c14a;
}
</style>
