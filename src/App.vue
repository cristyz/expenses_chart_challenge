<template>
  <main>
    <div class="my-balance fade-in">
      <div class="balance">
        <span>My balance</span>
        <strong>{{ formatMoney(myBalanceValue) }}</strong>
      </div>
      <img src="./assets/svgs/circles.svg" alt="Logo" />
    </div>
    <div class="my-chart fade-in delay-4">
      <h3>Spending - Last 7 days</h3>
      <div class="chart">
        <BaseDay
          v-for="day in days"
          :key="day.label"
          :day="day"
          :dayWithMaxValue="dayWithMaxValue!"
        />
      </div>

      <hr />

      <div class="total-month">
        <div class="total-month__display">
          <span>Total this month</span>
          <strong>{{ formatMoney(totalThisMonthValue) }}</strong>
        </div>
        <div class="total-month__details">
          <strong>+2.4%</strong>
          <span>from last month</span>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup lang="ts">
import { computed } from "vue";
import BaseDay from "./components/BaseDay.vue";
import { IDay } from "./interfaces/IDay";
import { formatMoney } from "./utils/money";

const days = computed(
  () =>
    <Array<IDay>>[
      { label: "mon", value: 24233 },
      { label: "tue", value: 16133 },
      { label: "wed", value: 32130 },
      { label: "thu", value: 43222 },
      { label: "fri", value: 26223 },
      { label: "sat", value: 12323 },
      { label: "sun", value: 22334 },
    ]
);

const dayWithMaxValue = computed(() => {
  const maxValue = Math.max(...days.value.map((day) => day.value));
  return days.value.find((day) => day.value === maxValue);
});

const totalThisMonthValue = computed(() => {
  return days.value.reduce((acc, day) => acc + day.value, 0);
});

const myBalanceValue = computed(() => {
  return totalThisMonthValue.value * 1.4;
});
</script>

<style scoped lang="scss">
main {
  min-height: 100vh;
  max-width: 500px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 1rem;

  .my-balance {
    background-color: $soft-red;
    padding: 1.5rem 2rem;
    border-radius: $border-radius;
    box-shadow: $box-shadow;
    width: 100%;

    display: flex;
    justify-content: space-between;

    .balance {
      display: flex;
      flex-direction: column;
      color: white;

      span {
        font-size: 0.8rem;
      }

      strong {
        font-size: 1.5rem;
        font-weight: 600;
      }
    }
  }

  .my-chart {
    background-color: white;
    padding: 1.5rem 2rem;
    border-radius: $border-radius;
    box-shadow: $box-shadow;
    width: 100%;

    display: flex;
    flex-direction: column;
    gap: 1rem;

    h3 {
      color: $dark-brown;
      font-size: 1.5rem;
    }

    hr {
      border: 0;
      height: 2px;
      background-color: $cream;
    }

    .total-month {
      display: flex;
      justify-content: space-between;
      align-items: flex-end;
      gap: 1rem;

      &__details,
      &__display {
        display: flex;
        flex-direction: column;

        span {
          font-size: 0.8rem;
          color: $medium-brown;
        }

        strong {
          font-weight: 600;
        }
      }

      &__display {
        strong {
          font-size: 1.8rem;
        }
      }

      &__details {
        text-align: end;

        strong {
          font-size: 0.9rem;
        }
      }
    }

    .chart {
      display: flex;
      gap: 1rem;

      height: 10rem;
    }
  }
}
</style>
