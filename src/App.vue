<template>
  <Header :title="title" />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="income" :expense="expense" />
    <TransactionList :transactions="transactions" @transaction-deleted="onTrxnDelete" />
    <AddTransaction @transaction-submitted="onTrxnSubmit" />
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

import { ref, computed, onMounted } from "vue";
import { useToast } from "vue-toastification";
const toast = useToast();

const title = "Expense Tracker";

const transactions = ref([]);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

// Get total
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  }, 0);
});

// Get income
const income = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

// Get expenses
const expense = computed(() => {
  return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

const onTrxnSubmit = (transaction) => {
  transactions.value.push({
    id: generateUniqId(),
    text: transaction.text,
    amount: transaction.amount
  });
  toast.success('Transaction added successfully!');
  saveToLocal();
}

const generateUniqId = () => Math.floor(Math.random() * 1000000);

const onTrxnDelete = (id) => {
  transactions.value = transactions.value.filter((trxn) => trxn.id !== id);
  toast.success('Transaction deleted successfully!');
  saveToLocal();
}

// Save to localStorage
const saveToLocal = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
}
</script>
