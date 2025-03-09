<template>
  <div class="container">
    <div class="content-wrapper">
      <div class="card elevation-0" style="background-color: white">
        <!-- Логотип (сверху посередине) и пользовательская информация -->
        <div class="header-wrapper d-flex justify-between align-center mb-4">
          <div class="logo-wrapper">
            <img
              src="../assets/logo.png"
              style="max-height: 70px; max-width: 170px"
              alt="AGTS Logo"
            />
          </div>
          <div class="user-info d-flex align-center mt-4">
            <div class="user-details">
              <div class="user-surname">Исмоилов</div>
              <div class="user-name">Абдукарим</div>
              <div class="user-phone">+7 (888) 999 10 10</div>
            </div>
            <img src="../assets/empty-user.png" class="user-avatar" alt="User Avatar" />
          </div>
        </div>
        <div class="title-bar mb-4">
          <div class="title-text">Для получения выплат введите данные</div>
        </div>

        <!-- Вложенная карточка для инпутов паспорта -->
        <div class="input-card mb-6">
          <!-- Поле для документа с меткой сверху слева -->
          <div class="input-label-wrapper mb-1">
            <div class="input-container d-flex justify-center">
              <div class="text">Данные паспорта</div>
              <div class="input-content">
                <label class="label-text">
                  <span class="required">*</span>
                </label>
                <select class="custom-select">
                  <option value="passport_rf">Паспорт РФ</option>
                  <option value="passport_foreign">Паспорт другой страны</option>
                  <option value="id_rf">Вид на жительство РФ</option>
                </select>
              </div>
            </div>
          </div>

          <!-- Поля для серии и номера в одной строке с метками сверху слева -->
          <div class="input-label-wrapper mb-2">
            <div class="input-container d-flex justify-center">
              <div
                class="input-content d-flex justify-between align-center series-number-row"
              >
                <div class="half-input">
                  <label class="label-text">
                    <span class="required">*</span>
                  </label>
                  <input type="text" placeholder="Серия" class="custom-input" />
                </div>
                <div class="half-input">
                  <label class="label-text">
                    <span class="required">*</span>
                  </label>
                  <input type="text" placeholder="Номер" class="custom-input" />
                </div>
              </div>
            </div>
          </div>

          <!-- Кнопка "Сохранить" для паспорта -->
        </div>

        <div class="button-wrapper mb-6 d-flex justify-center">
          <button class="submit-button" @click="savePassport">Сохранить</button>
        </div>

        <div class="title-bar mb-4">
          <div class="title-text">Введите номер карты для получения выплат</div>
        </div>

        <!-- Вложенная карточка для номера карты -->
        <div class="input-card mb-6">
          <!-- Поле для номера карты с меткой сверху слева -->
          <div class="input-label-wrapper mb-2">
            <div class="input-container d-flex justify-center">
              <div class="input-content">
                <label class="label-text">
                  <span class="required">*</span>
                </label>
                <input
                  type="text"
                  placeholder="1111 2222 4444 5555"
                  class="custom-input input-center"
                />
              </div>
            </div>
          </div>

          <!-- Поле для названия карты с меткой сверху слева -->
          <div class="input-label-wrapper mb-0">
            <div class="input-container d-flex justify-center">
              <div class="input-content">
                <input
                  type="text"
                  placeholder="Название"
                  class="custom-input input-center"
                />
              </div>
            </div>
          </div>

          <!-- Кнопка "Сохранить" для карты -->
        </div>
        <div class="button-wrapper mb-4 d-flex justify-center">
          <button class="submit-button" @click="saveCard">Сохранить</button>
        </div>

        <!-- Кнопка "Добавить карту" -->
        <div class="button-wrapper d-flex justify-center">
          <button class="back-button" @click="goBack">Добавить карту +</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();
const savePassport = () => {
  console.log("Passport data saved");
  // Логика сохранения данных паспорта
};
const saveCard = () => {
  console.log("Card data saved");
  // Логика сохранения данных карты
};
const goBack = () => {
  router.push("/");
};
</script>

<style scoped>
/* Базовые стили для контейнера */
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding: 0;
  background-color: white;
  width: 100%;
  margin: 0;
  box-sizing: border-box;
}

.text {
  color: rgba(0, 0, 0, 1);
  font-size: 13px;
}

/* Обёртка содержимого для точного центрирования и поднятия */
.content-wrapper {
  width: 100%;
  padding: 0 16px;
  margin-top: 20px;
}

/* Обертка для заголовка, растягиваем на всю ширину */
.title-bar {
  background-color: #01a7e3; /* Синий фон, как на макете */
  border-radius: 0; /* Убрал закругление, чтобы полоса выглядела прямо на всю ширину */
  margin: 0 -48px; /* Убираем отступы слева и справа */
  padding: 5px 0; /* Внутренние отступы, как на макете */
  width: calc(100% + 90px); /* Растягиваем на всю ширину контейнера */
}

.title-text {
  font-size: 13px;
  color: white;
  font-family: "Cabin", sans-serif;
  text-align: center; /* Центрируем текст внутри полосы */
}

/* Обертка для логотипа и информации пользователя */
.header-wrapper {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
}

.logo-wrapper {
  margin-top: 0;
}

.logo-wrapper img {
  max-height: 70px;
  max-width: 170px;
}

.user-info {
  text-align: right;
}

.user-details {
  margin-right: 8px;
}

.user-surname {
  font-size: 10px;
  color: rgba(0, 0, 0, 1);
  font-family: "Cabin", sans-serif;
  margin-bottom: 0.1vh;
}

.user-name {
  font-size: 10px;
  color: rgba(0, 0, 0, 0.5);
  font-family: "Cabin", sans-serif;
  margin-bottom: 0.1vh;
}

.user-phone {
  font-size: 8px;
  color: #616161;
  font-family: "Cabin", sans-serif;
  white-space: nowrap; /* Убеждаемся, что телефон не переносится */
}

.user-avatar {
  width: 37%;
  height: 37px;
  border-radius: 50%; /* Круглый аватар, как на макете */
  object-fit: cover;
}

/* Базовые стили для карточки */
.card {
  border-radius: 0;
  box-shadow: none;
  width: 100%;
}

/* Обертка для логотипа */
.logo-wrapper {
  margin-top: 40px;
}

/* Вложенная карточка для инпутов */
.input-card {
  border: 2px solid transparent;
  border-radius: 20px;
  padding: 16px;
  background-color: white;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.4);
  max-width: 340px;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 16px;
}

/* Обертка для метки и инпута */
.input-label-wrapper {
  display: flex;
  flex-direction: column;
  gap: 4px;
  width: 100%;
  min-height: 40px;
}

/* Контейнер для центрирования инпута */
.input-container {
  width: 100%;
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}

/* Контейнер для метки и инпута внутри */
.input-content {
  max-width: 246px;
  width: 100%;
  display: flex;
  flex-direction: column;
}

/* Стили для метки */
.label-text {
  font-size: 13px;
  color: #616161;
  font-weight: 500;
  align-self: flex-start;
  font-family: "Cabin", sans-serif;
  display: flex;
  flex-direction: column;
  height: 100%;
}

.required {
  color: red;
  align-self: flex-start;
  margin-top: auto;
  margin-bottom: -1vh;
  font-size: 13px;
}

/* Стили для кастомного инпута и селекта */
.custom-input,
.custom-select {
  border-radius: 10px;
  border: 2px solid #01a7e3 !important;
  background-color: white;
  height: 31px;
  padding: 0 8px;
  font-size: 14px;
  width: 100%;
  max-width: 246px;
  outline: none;
  box-sizing: border-box;
  text-align: left;
  font-family: "Cabin", sans-serif;
}

.input-center {
  text-align: center;
}

/* Стили для выпадающего списка селекта */
.custom-select {
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  background-image: url('data:image/svg+xml;utf8,<svg fill="%2301A7E3" height="24" viewBox="0 0 24 24" width="24" xmlns="http://www.w3.org/2000/svg"><path d="M7 10l5 5 5-5z"/><path d="M0 0h24v24H0z" fill="none"/></svg>');
  background-repeat: no-repeat;
  background-position: right 8px center;
  background-size: 20px;
}

.custom-select::-ms-expand {
  display: none;
}

.custom-select::after {
  content: "";
  position: absolute;
  right: 8px;
  top: 50%;
  transform: translateY(-50%);
  width: 12px;
  height: 12px;
  background-image: url('data:image/svg+xml;utf8,<svg fill="%2301A7E3" height="24" viewBox="0 0 24 24" width="24" xmlns="http://www.w3.org/2000/svg"><path d="M7 10l5 5 5-5z"/><path d="M0 0h24v24H0z" fill="none"/></svg>');
  background-repeat: no-repeat;
  background-size: contain;
  pointer-events: none;
}

.custom-select:focus + .select-options,
.custom-select:hover + .select-options {
  display: block;
}

.custom-select option {
  background-color: white;
  color: #000;
  border: 2px solid #01a7e3;
  border-radius: 10px;
  padding: 8px;
  margin: 2px 0;
}

/* Стили при фокусе на инпуте */
.custom-input:focus,
.custom-select:focus {
  border-color: #01a7e3;
  box-shadow: none;
}

/* Стили для двух полей (серия и номер) рядом */
.series-number-row {
  flex-direction: row; /* Убедимся, что поля идут в строку */
  width: 100%; /* Занимают всю доступную ширину */
}

.half-input {
  flex: 0 0 48%; /* Устанавливаем фиксированную ширину для каждого поля */
  margin-right: 4%; /* Отступ между полями */
}

.half-input:last-child {
  margin-right: 0; /* Убираем отступ для последнего поля */
}

/* Общие стили для кнопок */
.button-wrapper {
  display: flex;
  justify-content: center;
  margin-left: auto;
  margin-right: auto;
}

/* Стили для кнопки "Сохранить" */
.submit-button {
  border-radius: 10px;
  background-color: #01a7e3;
  color: white;
  border: none;
  padding: 6px 24px;
  font-size: 13px;
  cursor: pointer;
  width: 100%;
  max-width: 246px;
  height: 31px;
  font-family: "Cabin", sans-serif;
}

.submit-button:hover {
  background-color: #1976d2;
}

/* Стили для кнопки "Добавить карту" */
.back-button {
  border-radius: 10px;
  background-color: #01a7e3;
  color: white;
  border: none;
  padding: 6px 24px;
  font-size: 13px;
  cursor: pointer;
  width: 100%;
  max-width: 246px;
  height: 31px;
  font-family: "Cabin", sans-serif;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.back-button::after {
  margin-left: 8px;
  font-size: 18px;
  margin-top: -0.2vh;
}

.back-button:hover {
  background-color: #1976d2;
}

/* Адаптивность для разных устройств */

/* Мобильные устройства (390px и меньше) */
@media (max-width: 390px) {
  .input-content,
  .submit-button,
  .back-button {
    max-width: 246px;
  }

  .input-card {
    max-width: 300px;
  }

  .content-wrapper {
    padding: 0 8px;
    margin-top: 10px;
  }

  .card {
    padding: 16px;
  }

  .logo-wrapper {
    margin-top: 20px;
  }

  .series-number-row {
    flex-direction: row; /* Сохраняем поля в одну строку */
  }

  .half-input {
    width: 48%; /* Уменьшаем ширину для маленьких экранов, если нужно */
  }
}

/* Средние мобильные устройства и планшеты (391px - 768px) */
@media (min-width: 391px) and (max-width: 768px) {
  .input-content,
  .submit-button,
  .back-button {
    max-width: 246px;
  }

  .input-card {
    max-width: 330px;
  }

  .content-wrapper {
    padding: 0 16px;
    margin-top: 20px;
  }

  .card {
    padding: 20px;
  }

  .logo-wrapper {
    margin-top: 40px;
  }

  .series-number-row {
    flex-direction: row; /* Сохраняем поля в одну строку */
  }

  .half-input {
    width: 48%;
  }
}

/* Планшеты и небольшие десктопы (769px - 1023px) */
@media (min-width: 769px) and (max-width: 1023px) {
  .input-content,
  .submit-button,
  .back-button {
    max-width: 246px;
  }

  .input-card {
    max-width: 340px;
  }

  .content-wrapper {
    padding: 0 24px;
    margin-top: 30px;
  }

  .card {
    padding: 24px;
  }

  .logo-wrapper {
    margin-top: 50px;
  }

  .series-number-row {
    flex-direction: row; /* Сохраняем поля в одну строку */
  }

  .half-input {
    width: 48%;
  }
}

/* Десктопы (от 1024px и выше) */
@media (min-width: 1024px) {
  .input-content,
  .submit-button,
  .back-button {
    max-width: 246px;
  }

  .input-card {
    max-width: 350px;
  }

  .content-wrapper {
    padding: 0 32px;
    margin-top: 40px;
  }

  .card {
    padding: 32px;
  }

  .logo-wrapper {
    margin-top: 60px;
  }

  .series-number-row {
    flex-direction: row; /* Сохраняем поля в одну строку */
  }

  .half-input {
    width: 48%;
  }
}

/* Ландшафтная ориентация для мобильных устройств */
@media (max-width: 768px) and (orientation: landscape) {
  .container {
    min-height: 100vh;
    padding: 10px 0;
  }

  .input-content,
  .submit-button,
  .back-button {
    max-width: 246px;
  }

  .input-card {
    max-width: 240px;
  }

  .content-wrapper {
    margin-top: 10px;
  }

  .card {
    padding: 12px;
  }

  .logo-wrapper {
    margin-top: 20px;
  }

  .series-number-row {
    flex-direction: row; /* Сохраняем поля в одну строку */
  }

  .half-input {
    width: 48%;
  }
}

/* Утилитарные классы */
.d-flex {
  display: flex;
}

.justify-center {
  justify-content: center;
}

.align-center {
  align-items: center;
}

.justify-between {
  justify-content: space-between;
}

.mb-4 {
  margin-bottom: 16px;
}

.mb-6 {
  margin-bottom: 24px;
}
</style>
