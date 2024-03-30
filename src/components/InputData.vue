<script setup lang="ts">
import {ref} from "vue";
import CalendarWidget from "@/components/CalendarWidget/CalendarWidget.vue";

defineProps<{
  selectedLang: string
}>()

const selectedDate = ref<string>('');
const showCalendar = ref<boolean>(false);
</script>

<template>
  <div class="input-date">
    <input type="text" v-model="selectedDate"/>
    <div class="open-calendar" @click="() => showCalendar = !showCalendar"></div>
    <CalendarWidget
        :selected-lang="selectedLang"
        :date="selectedDate"
        @date-change="(date: string) => (selectedDate = date, showCalendar = false)"
        v-if="showCalendar" class="calendar"
    />
  </div>
</template>

<style scoped>
.input-date {
  margin: 1rem;
  display: flex;
  grid-template-columns: 10rem 1.2rem;
}
.open-calendar {
  border: 1px solid #000000;
  height: 1rem;
  width: 1rem;
  padding: .2rem;
  background-image: url("@/assets/images/calendar.svg");
  background-position: center;
  background-repeat: no-repeat;
  background-clip: padding-box;
  margin-left: -.1rem;
}

.open-calendar:hover {
  cursor: pointer;
}

.calendar {
  position: absolute;
  margin: 1.5rem 0 0 0;
}
</style>