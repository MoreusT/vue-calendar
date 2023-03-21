<script setup lang="ts">
import Day from './Day.vue'
import { DaysList, getWeekDays, daysOfMonth } from '../helpers/calendarHelper'
import { ref, watch } from 'vue'

const props = defineProps<{
  month: number,
  year: number
}>()

const daysInThisMonth = ref<DaysList>([]);
watch(props, newProps => daysInThisMonth.value = daysOfMonth(newProps.year, newProps.month), { immediate: true })
const weekDays = getWeekDays();

</script>

<template>
  <section class="calendar">
    <div class="wrapper">
      <ul class="day-name-list">
        <li class="day-name" v-for="name in weekDays">{{ name }}</li>
      </ul>
      <ul class="day-list">
        <Day v-for="({ value, currentMonth }, index) in daysInThisMonth" :dayNumber="value"
          :isCurrentMonth="currentMonth" />
      </ul>
    </div>
  </section>
</template>

<style scoped lang="scss">
@import "../assets/scss/utils/index.scss";

.wrapper {
  padding: rem(20px);
}

.day-name-list {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  grid-auto-rows: 1fr;
  padding-bottom: rem(10px);

  & .day-name {
    text-align: center;
    text-transform: capitalize;
    opacity: .6;
    font-size: rem(14px);
  }
}

.day-list {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  grid-auto-rows: 1fr;
  border: solid var(--light-bg-color);
  border-width: rem(1px) rem(1px) 0 0;

  @media (max-width: $mobile) {
    border-width: rem(1px) 0 0 0;
    padding: rem(10px);
    gap: rem(10px);
  }

  @media (max-width: $small-mobile) {
    gap: rem(3px);
  }
}
</style>