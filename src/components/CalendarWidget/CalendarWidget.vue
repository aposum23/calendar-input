<script setup lang="ts">
import {onMounted, ref, watch} from 'vue';
import { monthCode, daysCount } from "@/constants/dateCodes.ts";
import ArrowButton from "@/components/CalendarWidget/UI/ArrowButton.vue";
import names from '@/components/CalendarWidget/structures/names.ts';

const props = defineProps<{
  selectedLang: string;
  date?: string
}>();

const emit = defineEmits<{
  "date-change": [string];
}>()

const date = props.date ? props.date.split('-') : '';

const monthValue = ref<number>(1);
const yearValue = ref<number>(2024);
const dayValue = ref<number>();
const monthDays = ref<number[]>([]);

watch(
    () => dayValue.value,
    (newValue, oldValue) => {
        if (!!oldValue)
        emit('date-change',
            `${yearValue.value}-${monthValue.value > 9 ? monthValue.value : '0' + monthValue.value}-${dayValue.value > 9 ? dayValue.value : '0' + dayValue.value}`);
    }
)

const fillMonthDaysArray = () => {
  const lastYearNumbers = Number(String(yearValue.value).slice(2, 4));
  const isLeapYear = yearValue.value % 4 === 0;
  const yearCode = (6 + lastYearNumbers + Math.floor(lastYearNumbers / 4)) % 7;
  const month = isLeapYear && [1, 2].includes(monthValue.value) ? monthCode[monthValue.value - 1] - 1 : monthCode[monthValue.value - 1];
  const firstDayOffset = (6 + 1 + month + yearCode) % 7;
  monthDays.value.length = 0;
  let lastDate: number;
  if (monthValue.value === 2) lastDate = yearValue.value % 4 === 0 ? 29 : 28;
  else lastDate = daysCount[monthValue.value - 1]
  monthDays.value.length = 35;
  for (let i = 1; i <= lastDate; i++) {
    const weekDay = firstDayOffset === 6 ? i - 1 : i + firstDayOffset
    monthDays.value[weekDay] = i;
  }
}

const changeMonthValue = (event: number) => {
  if (monthValue.value + event === 0) {
    monthValue.value = 12
    yearValue.value -= 1;
  }
  else if (monthValue.value + event === 13) {
    monthValue.value = 1;
    yearValue.value += 1;
  }
  else monthValue.value += event;
  fillMonthDaysArray();
}

const selectDay = (dayNumber: number) => {
  dayValue.value = dayNumber;
}

onMounted(() => {
  if (props.date) {
    const date = props.date.split('-')
    dayValue.value = Number(date[2]);
    monthValue.value = Number(date[1]);
    yearValue.value = Number(date[0]);
  }
  else dayValue.value = 1;
  fillMonthDaysArray();
})
</script>

<template>
  <div class="wrapper">
    <ArrowButton arrow-direction="left" class="arrow-left" @change-month="changeMonthValue"/>
    <span class="month-year-title">{{`${names.MONTH_NAMES[selectedLang][monthValue - 1]} ${yearValue}`}}</span>
    <ArrowButton arrow-direction="right" class="arrow-right" @change-month="changeMonthValue"/>
    <template v-for="day in names.WEEK_DAYS_NAMES[selectedLang]" :key="day">
      <span class="week-day__name">{{day}}</span>
    </template>
    <template v-for="dayNumber in monthDays" :key="dayNumber">
      <span :class="`day-number ${dayValue === dayNumber ? 'current-date' : ''}`" @click="selectDay(dayNumber)">{{dayNumber}}</span>
    </template>
  </div>
</template>

<style scoped>
.wrapper {
  user-select: none;
  display: grid;
  grid-template-columns: repeat(7, 2rem);
  grid-template-rows: repeat(8, 2rem);
  border: 1px solid #7d7d7d;
  width: fit-content;
  background: #fff;
}

.arrow-left {
  grid-row: 1;
  grid-column: 1;
  margin: auto;
}

.arrow-right {
  grid-row: 1;
  grid-column: 7;
  margin: auto;
}

.month-year-title {
  grid-row: 1;
  grid-column: 2/7;
  text-align: center;
  font-size: 1.5rem;
  line-height: 2rem;
}

.week-day__name {
  text-align: center;
  line-height: 2rem;
  font-size: .9rem;
}

.current-date {
  border: 1px solid #65afdc !important;
}

.day-number {
  border: 1px solid transparent;
  text-align: center;
  line-height: 1.7rem;
}

.day-number:hover {
  cursor: pointer;
}
</style>