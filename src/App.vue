<script setup>
import { ref, computed } from "vue";

import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

const transactions = ref([
  { id: 1, text: "Flower", amount: -19.99 },
  { id: 2, text: "Salary", amount: 299.97 },
  { id: 3, text: "Book", amount: -10 },
  { id: 4, text: "Camera", amount: 150 },
  { id: 5, text: "Tech", amount: -400 },
  { id: 6, text: "Freelance", amount: 300 },
]);

const total = computed(() => {
  let total = 0;
  transactions.value.forEach((transaction) => {
    total += transaction.amount;
  });
  return Number(total.toFixed(2));
});

const incomes = computed(() => {
  let total = 0;
  transactions.value.forEach((transaction) => {
    if (transaction.amount > 0) {
      total += transaction.amount;
    }
  });
  return Number(total.toFixed(2));
});

const expenses = computed(() => {
  let total = 0;
  transactions.value.forEach((transaction) => {
    if (transaction.amount < 0) {
      total += transaction.amount;
    }
  });
  return Number(total.toFixed(2));
});
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :incomes="incomes" :expenses="expenses" />
    <TransactionList :transactions="transactions" />
    <AddTransaction />
  </div>
</template>
