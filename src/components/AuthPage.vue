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

        <!-- Поле для телефона с меткой сверху слева -->
        <div class="input-label-wrapper mb-4">
          <div class="input-container d-flex justify-center">
            <div class="input-content">
              <label class="label-text">Телефон</label>
              <input
                v-model="phone"
                type="tel"
                placeholder="+7 (___) ___-__-__"
                class="custom-input"
              />
            </div>
          </div>
        </div>

        <!-- Поле для пароля из SMS с меткой сверху слева -->
        <div class="input-label-wrapper mb-2">
          <div class="input-container d-flex justify-center">
            <div class="input-content">
              <label class="label-text">Пароль из SMS</label>
              <input
                v-model="smsCode"
                type="text"
                placeholder="Введите пароль"
                class="custom-input"
              />
            </div>
          </div>
        </div>

        <!-- Ссылка "Выслать новый пароль" -->
        <div class="link-wrapper mb-6 d-flex justify-center">
          <a href="#" class="link-text">Выслать новый пароль</a>
        </div>

        <!-- Кнопка "Войти" -->
        <div class="button-wrapper mb-3 d-flex justify-center">
          <button class="login-button" @click="handleLogin">ВОЙТИ</button>
        </div>

        <!-- Ссылка "Регистрация" -->
        <div class="link-wrapper d-flex justify-center">
          <a href="#" class="registration-text" @click.prevent="goToRegistration">Регистрация</a>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import router from '@/router'
import { ref } from 'vue'

// Реактивные переменные для данных формы
const phone = ref('')
const smsCode = ref('')

// Обработчик клика по кнопке "Войти" (можно расширить для логики авторизации)
const handleLogin = () => {
  console.log('Login attempt with:', { phone: phone.value, smsCode: smsCode.value })
  // Здесь добавьте вашу логику авторизации
}
const goToRegistration = () => {
  router.push('/r')
}
</script>

<style scoped>
/* Базовые стили для контейнера */
.container {
  min-height: 100vh; /* Занимаем всю высоту экрана */
  display: flex;
  justify-content: center; /* Центрируем по горизонтали */
  align-items: flex-start; /* Изменил на flex-start, чтобы поднять содержимое выше */
  padding: 0;
  background-color: white; /* Убедимся, что фон белый на всём экране */
  width: 100%;
  margin: 0;
  box-sizing: border-box;
}

/* Обёртка содержимого для точного центрирования и поднятия */
.content-wrapper {
  width: 100%;
  max-width: 400px; /* Уменьшаем максимальную ширину карточки для лучшего контроля */
  padding: 0 16px; /* Небольшие горизонтальные отступы на мобильных */
  margin-top: 20px; /* Добавляем верхний отступ, чтобы поднять содержимое выше */
}

/* Базовые стили для карточки */
.card {
  border-radius: 0;
  box-shadow: none;
  width: 100%; /* Карточка занимает всю доступную ширину */
}

/* Обертка для логотипа */
.logo-wrapper {
  margin-top: 40px; /* Увеличиваем верхний отступ, чтобы поднять логотип выше */
}

/* Обертка для метки и инпута */
.input-label-wrapper {
  display: flex;
  flex-direction: column;
  gap: 4px; /* Небольшой отступ между меткой и полем */
  width: 100%; /* Убеждаемся, что обёртка занимает всю ширину */
}

/* Контейнер для центрирования инпута */
.input-container {
  width: 100%;
  display: flex;
  justify-content: center; /* Центрируем содержимое по горизонтали */
}

/* Контейнер для метки и инпута внутри */
.input-content {
  max-width: 300px; /* Единая максимальная ширина для всех инпутов */
  width: 100%; /* Полная ширина внутри контейнера */
}

/* Стили для метки */
.label-text {
  font-size: 10px;
  color: #616161;
  font-weight: 500;
  align-self: flex-start;
  margin-left: 16px; /* Отступ слева для метки */
  font-family: 'Cabin', sans-serif; /* Применяем шрифт Cabin */
}

/* Стили для кастомного инпута */
.custom-input {
  border-radius: 10px; /* Закругленные углы, как на макете */
  border: 2px solid #01A7E3; /* Синяя обводка, как на макете */
  background-color: white;
  height: 30px; /* Фиксированная высота, как указано */
  padding: 0 8px; /* Минимальные внутренние отступы для текста */
  font-size: 14px; /* Размер текста внутри инпута, чтобы текст был читаемым */
  width: 100%; /* Полная ширина внутри контейнера */
  outline: none; /* Убираем стандартный контур при фокусе */
  box-sizing: border-box; /* Учитываем padding и border в общей ширине */
  text-align: center; /* Центрируем текст плейсхолдера и введённый текст */
  font-family: 'Cabin', sans-serif; /* Применяем шрифт Cabin */
}

/* Стили при фокусе на инпуте */
.custom-input:focus {
  border-color: #01A7E3; /* Сохраняем цвет обводки при фокусе */
  box-shadow: none; /* Убираем тень, если не нужна */
}

/* Общие стили для отступов */
.mb-4 {
  margin-bottom: 16px;
}

/* Обёртка для ссылок и кнопки */
.link-wrapper,
.button-wrapper {
  display: flex;
  justify-content: center; /* Центрируем по горизонтали */
  max-width: 300px; /* Единая максимальная ширина для всех элементов ниже */
  margin-left: auto;
  margin-right: auto; /* Центрируем внутри обёртки */
}

/* Стили для ссылки "Выслать новый пароль" */
.link-text {
  font-size: 13px;
  color: #5159FF; /* Синий цвет, как на макете */
  text-decoration: none; /* Убираем подчёркивание */
  font-family: 'Cabin', sans-serif; /* Применяем шрифт Cabin */
}

/* Стили для кнопки "Войти" */
.login-button {
  border-radius: 10px; /* Закругленные углы, как на макете */
  background-color: #01A7E3; /* Синий цвет фона, как на макете */
  color: white; /* Белый текст */
  border: none; /* Убираем границу */
  padding: 8px 24px; /* Внутренние отступы для кнопки */
  font-size: 10px; /* Размер текста, как на макете */
  cursor: pointer; /* Курсор указывает на кликабельность */
  width: 100%; /* Полная ширина внутри контейнера */
  max-width: 300px; /* Единая максимальная ширина */
  font-family: 'Cabin', sans-serif; /* Применяем шрифт Cabin */
}

/* Стили при наведении на кнопку */
.login-button:hover {
  background-color: #1976D2; /* Более тёмный синий при наведении */
}

/* Стили для ссылки "Регистрация" */
.registration-text {
  font-size: 13px;
  color: #5159FF; /* Фиолетовый цвет, как на макете */
  text-decoration: none; /* Убираем подчёркивание */
  font-family: 'Cabin', sans-serif; /* Применяем шрифт Cabin */
}

/* Адаптивность для разных устройств */

/* Мобильные устройства (например, iPhone 14, 390px и меньше) */
@media (max-width: 390px) {
  .input-content,
  .login-button {
    max-width: 250px; /* Фиксированная ширина для небольших экранов */
  }

  .content-wrapper {
    padding: 0 8px; /* Уменьшаем горизонтальные отступы для компактности */
    margin-top: 10px; /* Уменьшаем верхний отступ для мобильных */
  }

  .card {
    padding: 16px; /* Уменьшаем внутренние отступы для мобильных */
  }

  .logo-wrapper {
    margin-top: 20px; /* Уменьшаем верхний отступ логотипа на мобильных */
  }
}

/* Средние мобильные устройства и планшеты в портретной ориентации (391px - 768px) */
@media (min-width: 391px) and (max-width: 768px) {
  .input-content,
  .login-button {
    max-width: 280px; /* Увеличиваем ширину для средних экранов */
  }

  .content-wrapper {
    padding: 0 16px; /* Сохраняем горизонтальные отступы */
    margin-top: 20px; /* Сохраняем верхний отступ */
  }

  .card {
    padding: 20px; /* Увеличиваем внутренние отступы для планшетов */
  }

  .logo-wrapper {
    margin-top: 40px; /* Сохраняем верхний отступ логотипа */
  }
}

/* Планшеты и небольшие десктопы (769px - 1023px) */
@media (min-width: 769px) and (max-width: 1023px) {
  .input-content,
  .login-button {
    max-width: 300px; /* Устанавливаем единую ширину */
  }

  .content-wrapper {
    padding: 0 24px; /* Увеличиваем горизонтальные отступы */
    margin-top: 30px; /* Увеличиваем верхний отступ для планшетов */
  }

  .card {
    padding: 24px; /* Увеличиваем внутренние отступы для планшетов */
  }

  .logo-wrapper {
    margin-top: 50px; /* Увеличиваем верхний отступ логотипа для планшетов */
  }
}

/* Десктопы (от 1024px и выше) */
@media (min-width: 1024px) {
  .input-content,
  .login-button {
    max-width: 300px; /* Единая максимальная ширина для больших экранов */
  }

  .content-wrapper {
    padding: 0 32px; /* Увеличиваем горизонтальные отступы для десктопов */
    margin-top: 40px; /* Увеличиваем верхний отступ для десктопов */
  }

  .card {
    padding: 32px; /* Увеличиваем внутренние отступы для десктопов */
  }

  .logo-wrapper {
    margin-top: 60px; /* Увеличиваем верхний отступ логотипа для десктопов */
  }
}

/* Ландшафтная ориентация для мобильных устройств */
@media (max-width: 768px) and (orientation: landscape) {
  .container {
    min-height: 100vh; /* Сохраняем высоту экрана */
    padding: 10px 0; /* Добавляем небольшие отступы по вертикали */
  }

  .input-content,
  .login-button {
    max-width: 200px; /* Уменьшаем ширину в ландшафтной ориентации */
  }

  .content-wrapper {
    margin-top: 10px; /* Уменьшаем верхний отступ для ландшафтной ориентации */
  }

  .card {
    padding: 12px; /* Уменьшаем внутренние отступы для ландшафтной ориентации */
  }

  .logo-wrapper {
    margin-top: 20px; /* Уменьшаем верхний отступ логотипа в ландшафтной ориентации */
  }
}

/* Стили для логотипа и отступов */
.d-flex {
  display: flex;
}

.justify-center {
  justify-content: center;
}
</style>