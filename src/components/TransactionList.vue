<template>
  <h3>History</h3>
  <ul v-if="length > 0" id="list" class="list">
    <li v-for="(trxn, index) in transactions" :key="trxn.id" :class="trxn.amount < 0 ? 'minus': 'plus'">
        <span>{{ trxn.text }}</span>
        <span><span>&#8377;</span>{{ trxn.amount }}</span>
        <button class="delete-btn" @click="onDelete(trxn.id)">x</button>
    </li>
  </ul>
  <p v-else>No transactions added.</p>
</template>

<script setup>
import { defineProps, computed, defineEmits } from 'vue';
const props = defineProps({
  transactions: {
    type: Array,
    required: true
  }
});

const emit = defineEmits(['transactionDeleted'])

const onDelete = (id) => {
  emit('transactionDeleted', id)
}
const length = computed(() => props.transactions.length);
</script>
