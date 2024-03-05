<template>
  <div class="inc-exp-container">
    <div>
      <h4>Income</h4>
      <p id="money-plus" class="money plus"><span>&#8377;</span>{{ income }}</p>
    </div>
    <div>
      <h4>Expense</h4>
      <p id="money-minus" class="money minus"><span>&#8377;</span>{{ expense }}</p>
    </div>
  </div>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps({
  income: {
    type: String,
    required: true
  },
  expense: {
    type: String,
    required: true
  }
});
</script>
