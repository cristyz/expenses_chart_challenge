<template>
  <div class="day">
    <div
      class="day__bar day-height-animation delay-10"
      :data-value="dayValueFormated"
      :style="{ height: heightPercentage }"
    ></div>
    <span class="day__label">{{ day.label }}</span>
  </div>
</template>
<script setup lang="ts">
import { computed } from "vue";
import { IDay } from "../interfaces/IDay";
import { formatMoney } from "../utils/money";

const props = defineProps<{ day: IDay; dayWithMaxValue: IDay }>();

const heightPercentage = computed(() => {
  const percentage =
    (props.day.value / props.dayWithMaxValue.value) * 100 * 0.8;
  return `${percentage}%`;
});

const dayValueFormated = computed(() => formatMoney(props.day.value));
</script>
<style lang="scss" scoped>
.day {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  align-items: center;
  gap: 0.5rem;

  &__label {
    font-size: 0.8rem;
    color: $medium-brown;
  }

  &__bar {
    width: 100%;
    height: 20%;
    background-color: $soft-red;
    border-radius: 0.4rem;
    position: relative;

    transition: 0.2s ease-in-out;
    cursor: pointer;

    &:hover {
      background-color: $cyan;
      &::after {
        top: -2.5rem;
        opacity: 1;
      }
    }

    &::after {
      content: attr(data-value);
      position: absolute;
      opacity: 0;
      top: 0rem;
      left: 50%;
      transform: translateX(-50%);
      font-size: 0.8rem;
      color: white;
      border-radius: inherit;
      padding: 0.2rem 0.5rem;
      z-index: 9999;
      pointer-events: none;

      min-width: 100%;
      height: 25px;
      display: flex;
      justify-content: center;
      align-items: center;
      background-color: $dark-brown;
      transition: 0.2s ease-in-out;
    }
  }
}
</style>
