<template>
    <div class="container">
      <div class="content-wrapper">
        <div class="card elevation-0 pa-6" style="background-color: white;">
          <!-- Логотип -->
          <div class="logo-wrapper mb-4 d-flex justify-center">
            <img
              src="../assets/logo.png"
              style="max-height: 100px; max-width: 200px;"
              alt="AGTS Logo"
            />
          </div>
  
          <!-- Вложенная карточка для инпутов -->
          <div class="input-card mb-6">
            <div class="title-wrapper mb-0 d-flex justify-center">
              <div class="title-text">Регистрация</div>
            </div>
            <!-- Поле для телефона с меткой сверху слева -->
            <div class="input-label-wrapper mb-4">
              <div class="input-container d-flex justify-center">
                <div class="input-content">
                  <label class="label-text">
                    <span class="required">*</span>
                  </label>
                  <input
                    v-model="phone"
                    type="tel"
                    placeholder="+7 (___) ___-__-__"
                    class="custom-input-phone"
                  />
                </div>
              </div>
            </div>
  
            <!-- Поле для фамилии с меткой сверху слева -->
            <div class="input-label-wrapper mb-0">
              <div class="input-container d-flex justify-center">
                <div class="input-content">
                  <label class="label-text">
                    <span class="required">*</span>
                  </label>
                  <input
                    v-model="familyName"
                    type="text"
                    placeholder="Фамилия"
                    class="custom-input"
                  />
                </div>
              </div>
            </div>
  
            <!-- Поле для имени с меткой сверху слева -->
            <div class="input-label-wrapper mb-4">
              <div class="input-container d-flex justify-center">
                <div class="input-content">
                  <label class="label-text">
                    <span class="required">*</span>
                  </label>
                  <input
                    v-model="name"
                    type="text"
                    placeholder="Имя"
                    class="custom-input"
                  />
                </div>
              </div>
            </div>
  
            <!-- Поле для отчества с меткой сверху слева -->
            <div class="input-label-wrapper mb-4">
              <div class="input-container d-flex justify-center">
                <div class="input-content">
                  <input
                    v-model="patronymic"
                    type="text"
                    placeholder="Отчество"
                    class="custom-input"
                  />
                </div>
              </div>
            </div>
          </div>
  
          <!-- Текст уведомления -->
          <div class="notification-wrapper mb-4 d-flex justify-center">
            <div class="d-flex justify-center">
              <p class="notification-text">
                Я согласен на обработку
                <a href="/personal-data" class="link-text">персональных данных</a>,
                с правилами пользования и договором
                <a href="/offer" class="link-text">оферты</a>.
              </p>
            </div>
          </div>
  
          <!-- Кнопка "Согласись" -->
          <div class="button-wrapper mb-3 d-flex justify-center">
            <button class="submit-button" @click="handleSubmit">Согласен</button>
          </div>
  
          <!-- Дополнительные опции -->
          <div class="options-wrapper mb-4 d-flex flex-column align-center">
            <button
              class="option-button mb-2"
              @click="getPassword"
              :class="{ 'active-option': isAgreed }"
              :disabled="!isAgreed"
            >
              ПОЛУЧИТЬ ПАРОЛЬ
            </button>
            <transition name="fade">
              <div v-if="showCodeInput" class="input-label-wrapper mb-4">
                <div class="input-container">
                  <div class="sms-text">Пароль будет сгенерирован автоматически и поступит в SMS</div>
                  <div class="input-content">
                    <input
                      v-model="smsCode"
                      type="text"
                      class="custom-input"
                    />
                  </div>
                </div>
              </div>
            </transition>
            <button class="option-button" :disabled="smsCode.length >= 4" :class="{ 'active-option': smsCode.length >= 4 }">ЗАРЕГИСТРИРОВАТЬСЯ</button>
          </div>
  
          <!-- Кнопка "Назад" -->
          <div class="back-wrapper d-flex justify-center">
            <button class="back-button" @click="goBack">
              <span class="arrow">←</span> Назад
            </button>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue'
  import { useRouter } from 'vue-router'
  
  // Реактивные переменные для данных формы
  const phone = ref('')
  const familyName = ref('')
  const name = ref('')
  const patronymic = ref('')
  const smsCode = ref('') // Новая переменная для кода
  const isAgreed = ref(false) // Состояние согласия
  const showCodeInput = ref(false) // Состояние видимости нового инпута
  
  const router = useRouter()
  
  // Обработчик клика по кнопке "Согласен"
  const handleSubmit = () => {
    isAgreed.value = true // Устанавливаем состояние согласия
    console.log('Registration attempt with:', {
      phone: phone.value,
      familyName: familyName.value,
      name: name.value,
      patronymic: patronymic.value,
    })
    // Здесь добавьте вашу логику регистрации
  }
  
  // Обработчик клика по кнопке "Получить пароль"
  const getPassword = () => {
    showCodeInput.value = true // Показываем новый инпут
    console.log('Password request with SMS code:', smsCode.value)
    // Здесь добавьте логику получения пароля
  }
  
  // Обработчик клика по кнопке "Назад"
  const goBack = () => {
    router.push('/')
  }
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
  
  /* Обёртка содержимого для точного центрирования и поднятия */
  .content-wrapper {
    width: 100%;
    max-width: 400px;
    padding: 0 16px;
    margin-top: 20px;
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
  
  /* Обертка для заголовка */
  .title-wrapper {
    margin-bottom: 0; /* Убрал отступ для тесного прилегания */
  }
  
  .title-text {
    font-size: 13px;
    color: #000;
    font-family: 'Cabin', sans-serif;
  }
  
  /* Вложенная карточка для инпутов */
  .input-card {
    border: 2px solid transparent; /* Прозрачная обводка для эффекта */
    border-radius: 20px;
    padding: 16px;
    background-color: white;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.4); /* Тень снизу для эффекта поднятия */
    max-width: 340px; /* Увеличена ширина карточки */
    margin-left: auto;
    margin-right: auto;
  }
  
  /* Обертка для метки и инпута */
  .input-label-wrapper {
    display: flex;
    flex-direction: column;
    gap: 4px;
    width: 100%;
    min-height: 40px; /* Установили минимальную высоту для размещения * внизу */
  }
  
  /* Контейнер для центрирования инпута */
  .input-container {
    width: 100%;
    display: flex;
    justify-content: center;
    flex-direction: column; /* Изменили на колонку для вертикального расположения */
    align-items: center; /* Центрируем содержимое по горизонтали */
  }
  
  /* Контейнер для метки и инпута внутри */
  .input-content {
    max-width: 250px;
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
    font-family: 'Cabin', sans-serif;
    display: flex;
    flex-direction: column;
    height: 100%; /* Занимает всю доступную высоту */
  }
  
  .required {
    color: red;
    align-self: flex-end; /* Выравнивание по нижнему краю */
    margin-top: auto; /* Отодвигает * вниз */
    margin-bottom: -1vh;
    font-size: 13px; /* Немного уменьшил размер для компактности */
  }
  
  /* Стили для кастомного инпута */
  .custom-input {
    border-radius: 10px;
    border: 2px solid #01A7E3;
    background-color: white;
    height: 31px; /* Исправлено на 31px согласно макету */
    padding: 0 8px;
    font-size: 14px;
    width: 100%;
    max-width: 246px; /* Установлена ширина согласно макету */
    outline: none;
    box-sizing: border-box;
    text-align: left;
    font-family: 'Cabin', sans-serif;
  }
  
  .custom-input-phone {
    border-radius: 10px;
    border: 2px solid #01A7E3;
    background-color: white;
    height: 31px; /* Исправлено на 31px согласно макету */
    padding: 0 8px;
    font-size: 14px;
    width: 100%;
    max-width: 246px; /* Установлена ширина согласно макету */
    outline: none;
    box-sizing: border-box;
    text-align: center;
    font-family: 'Cabin', sans-serif;
  }
  
  /* Стили при фокусе на инпуте */
  .custom-input:focus {
    border-color: #01A7E3;
    box-shadow: none;
  }
  
  /* Общие стили для отступов */
  .mb-4 {
    margin-bottom: 16px;
  }
  
  .mb-2 {
    margin-bottom: 8px;
  }
  
  .mb-0 {
    margin-bottom: 0;
  }
  
  /* Обёртка для уведомления */
  .notification-wrapper {
    text-align: center;
    position: relative;
    padding-bottom: 8px; /* Добавлен отступ снизу для линии */
  }
  
  .sms-text {
    font-size: 10px;
    color: #949494;
    font-family: 'Cabin', sans-serif;
    line-height: 17px; /* Небольшой межстрочный интервал */
    max-width: 200px; /* Ограничение ширины текста */
    text-align: center;
    align-items: center;
    justify-content: center;
    display: flex;
    margin-bottom: 2vh;
  }
  
  .notification-text {
    font-size: 11px;
    color: #949494;
    font-family: 'Cabin', sans-serif;
    line-height: 17px; /* Небольшой межстрочный интервал */
    max-width: 300px; /* Ограничение ширины текста */
    margin: 0 auto;
    text-align: center; /* Убедимся, что текст выровнен по центру */
  }
  
  /* Стили для ссылок в тексте уведомления */
  .link-text {
    color: #01A7E3; /* Цвет ссылок, как на макете */
    text-decoration: none; /* Убираем подчеркивание */
    font-weight: 500; /* Немного выделяем текст */
  }
  
  .link-text:hover {
    text-decoration: underline; /* Подчеркивание при наведении */
  }
  
  /* Горизонтальная линия под текстом */
  .underline {
    width: 100%;
    height: 1px;
    background-color: #01A7E3; /* Синяя линия, как на макете */
    position: absolute;
    bottom: 0;
    left: 0;
    max-width: 300px;
    margin: 0 auto;
  }
  
  /* Обёртка для кнопки и ссылок */
  .button-wrapper,
  .back-wrapper,
  .options-wrapper {
    display: flex;
    justify-content: center;
    margin-left: auto;
    margin-right: auto;
  }
  
  /* Стили для кнопки "Согласись" */
  .submit-button {
    border-radius: 10px;
    background-color: #01A7E3;
    color: white;
    border: none;
    padding: 6px 24px;
    font-size: 13px;
    cursor: pointer;
    width: 100%;
    max-width: 246px; 
    height: 31px; 
    font-family: 'Cabin', sans-serif;
  }
  
  .submit-button:hover {
    background-color: #1976D2;
  }
  
  /* Стили для дополнительных опций */
  .option-button {
    border-radius: 10px;
    background-color: #CDCDCD;
    color: #fff;
    border: none;
    padding: 8px 24px;
    font-size: 10px;
    cursor: pointer;
    width: 100%;
    max-width: 246px; /* Установлена ширина согласно макету */
    height: 31px; /* Исправлено на 31px согласно макету */
    font-family: 'Cabin', sans-serif;
  }
  
  .option-button.active-option {
    background-color: #01A7E3; /* Цвет при активном состоянии */
  }
  
  .option-button:hover {
    background-color: #B0B0B0;
  }
  
  /* Стили для кнопки "Назад" */
  .back-button {
    border-radius: 10px;
    background-color: #01A7E3;
    color: white;
    border: none;
    padding: 8px 24px;
    font-size: 10px;
    cursor: pointer;
    width: 100%;
    max-width: 246px; /* Установлена ширина согласно макету */
    height: 31px; /* Исправлено на 31px согласно макету */
    font-family: 'Cabin', sans-serif;
    position: relative; /* Для позиционирования стрелки */
    display: flex;
    align-items: center; /* Вертикальное выравнивание */
    justify-content: center; /* Горизонтальное центрирование содержимого */
    overflow: hidden; /* Убираем лишнее за пределами кнопки */
  }
  
  .back-button .arrow {
    color: white; /* Белый цвет стрелки */
    position: absolute; /* Абсолютное позиционирование стрелки */
    left: 8px; /* Отступ от левого края */
    font-size: 18px; /* Немного увеличим размер стрелки для видимости */
    margin-top: -.2vh;
  }
  
  .back-button span.text {
    margin: 0 auto; /* Центрируем текст */
    display: inline-block; /* Для корректного центрирования */
  }
  
  
  .back-button:hover {
    background-color: #1976D2;
  }
  
  /* Адаптивность для разных устройств */
  
  /* Мобильные устройства (390px и меньше) */
  @media (max-width: 390px) {
    .input-content,
    .submit-button,
    .option-button,
    .back-button {
      max-width: 246px; /* Сохранена ширина согласно макету */
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
  }
  
  /* Средние мобильные устройства и планшеты (391px - 768px) */
  @media (min-width: 391px) and (max-width: 768px) {
    .input-content,
    .submit-button,
    .option-button,
    .back-button {
      max-width: 246px; /* Сохранена ширина согласно макету */
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
  }
  
  /* Планшеты и небольшие десктопы (769px - 1023px) */
  @media (min-width: 769px) and (max-width: 1023px) {
    .input-content,
    .submit-button,
    .option-button,
    .back-button {
      max-width: 246px; /* Сохранена ширина согласно макету */
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
  }
  
  /* Десктопы (от 1024px и выше) */
  @media (min-width: 1024px) {
    .input-content,
    .submit-button,
    .option-button,
    .back-button {
      max-width: 246px; /* Сохранена ширина согласно макету */
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
  }
  
  /* Ландшафтная ориентация для мобильных устройств */
  @media (max-width: 768px) and (orientation: landscape) {
    .container {
      min-height: 100vh;
      padding: 10px 0;
    }
  
    .input-content,
    .submit-button,
    .option-button,
    .back-button {
      max-width: 246px; /* Сохранена ширина согласно макету */
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
  }
  
  /* Стили для логотипа и отступов */
  .d-flex {
    display: flex;
  }
  
  .justify-center {
    justify-content: center;
  }
  
  .align-center {
    align-items: center;
  }
  
  .flex-column {
    flex-direction: column;
  }
  </style>