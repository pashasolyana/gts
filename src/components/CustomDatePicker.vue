<template>
  <div class="custom-date-picker" :class="{ open: isOpen }">
    <input
      type="text"
      :value="formattedDate"
      @click="toggleCalendar"
      readonly
      class="date-input"
    />
    <div v-if="isOpen" class="calendar">
      <div class="calendar-header">
        <button @click="changeMonth(-1)" class="nav-btn">&lt;</button>
        <span class="month-year">{{ months[currentMonth] }} {{ currentYear }}</span>
        <button @click="changeMonth(1)" class="nav-btn">&gt;</button>
      </div>
      <div class="calendar-weekdays">
        <span v-for="day in weekdays" :key="day">{{ day }}</span>
      </div>
      <div class="calendar-days">
        <span
          v-for="day in days"
          :key="day.date"
          :class="{ current: isCurrentDay(day.date), selected: isSelectedDay(day.date) }"
          @click="selectDate(day.date)"
        >
          {{ day.day }}
        </span>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";

export default {
  name: "CustomDatePicker",
  props: {
    modelValue: {
      type: String,
      default: null,
    },
  },
  setup(props, { emit }) {
    const isOpen = ref(false);
    const currentDate = ref(new Date());
    const selectedDate = ref(props.modelValue ? new Date(props.modelValue) : null);
    const months = [
      "Январь",
      "Февраль",
      "Март",
      "Апрель",
      "Май",
      "Июнь",
      "Июль",
      "Август",
      "Сентябрь",
      "Октябрь",
      "Ноябрь",
      "Декабрь",
    ];
    const weekdays = ["Пн", "Вт", "Ср", "Чт", "Пт", "Сб", "Вс"];

    const currentMonth = computed(() => currentDate.value.getMonth());
    const currentYear = computed(() => currentDate.value.getFullYear());

    const days = computed(() => {
      const year = currentYear.value;
      const month = currentMonth.value;
      const firstDay = new Date(year, month, 1);
      const lastDay = new Date(year, month + 1, 0);
      const daysInMonth = lastDay.getDate();
      const startingDay = firstDay.getDay() === 0 ? 7 : firstDay.getDay(); // 1 (Пн) -> 7 (Вс)

      const daysArray = [];
      // Добавить пустые ячейки до начала месяца
      for (let i = 1; i < startingDay; i++) {
        daysArray.push({ day: "", date: null });
      }
      // Добавить дни месяца
      for (let day = 1; day <= daysInMonth; day++) {
        const date = new Date(year, month, day);
        daysArray.push({ day, date });
      }
      // Добавить пустые ячейки до конца недели
      while (daysArray.length % 7 !== 0) {
        daysArray.push({ day: "", date: null });
      }

      return daysArray;
    });

    const formattedDate = computed(() => {
      if (!selectedDate.value) return "";
      const day = String(selectedDate.value.getDate()).padStart(2, "0");
      const month = String(selectedDate.value.getMonth() + 1).padStart(2, "0");
      const year = selectedDate.value.getFullYear();
      return `${day}.${month}.${year}`;
    });

    const toggleCalendar = () => {
      isOpen.value = !isOpen.value;
    };

    const changeMonth = (delta) => {
      const newMonth = currentMonth.value + delta;
      const newYear = currentYear.value + Math.floor(newMonth / 12);
      const adjustedMonth = ((newMonth % 12) + 12) % 12; // Корректировка для отрицательных месяцев
      currentDate.value = new Date(newYear, adjustedMonth, 1);
      console.log(`Changed to: ${months[adjustedMonth]} ${newYear}`); // Для отладки
    };

    const selectDate = (date) => {
      if (date) {
        selectedDate.value = new Date(date);
        isOpen.value = false;
        emit("update:modelValue", selectedDate.value.toISOString().split("T")[0]);
      }
    };

    const isCurrentDay = (date) => {
      if (!date) return false;
      const today = new Date();
      return date.toDateString() === today.toDateString();
    };

    const isSelectedDay = (date) => {
      if (!date || !selectedDate.value) return false;
      return date.toDateString() === selectedDate.value.toDateString();
    };

    return {
      isOpen,
      currentDate,
      selectedDate,
      months,
      weekdays,
      currentMonth,
      currentYear,
      days,
      formattedDate,
      toggleCalendar,
      changeMonth,
      selectDate,
      isCurrentDay,
      isSelectedDay,
    };
  },
};
</script>

<style scoped>
.custom-date-picker {
  position: relative;
  width: 100%;
}

.date-input {
  width: 100%;
  padding: 8px;
  border: 1px solid #01a7e3;
  border-radius: 10px;
  font-size: 14px;
  background-color: transparent;
  cursor: pointer;
  box-sizing: border-box;
}

.calendar {
  position: absolute;
  top: 100%;
  left: 0;
  z-index: 1000;
  background-color: white;
  border: 1px solid #e0e0e0;
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  padding: 8px;
  width: 250px;
}

.calendar-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
  font-size: 14px;
  color: #333;
}

.nav-btn {
  background: none;
  border: none;
  font-size: 16px;
  cursor: pointer;
  padding: 0 4px;
  color: #01a7e3;
}

.month-year {
  font-weight: bold;
}

.calendar-weekdays {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  text-align: center;
  font-size: 12px;
  color: #666;
  margin-bottom: 4px;
}

.calendar-days {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  text-align: center;
}

.calendar-days span {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 30px;
  height: 30px;
  margin: 2px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 14px;
}

.calendar-days span.current {
  color: #01a7e3;
}

.calendar-days span.selected {
  background-color: #01a7e3;
  color: white;
}

.calendar-days span:hover {
  background-color: #e0f7fa;
}
</style>
