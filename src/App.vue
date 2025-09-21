<script setup>
import { ref, computed, onMounted } from "vue";

import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { useToast } from "vue-toastification";

const transactions = ref([]);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transactions"));

  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

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

const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
};

const handleTransactionSubmitted = (form) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: form.text,
    amount: Number(form.amount),
  });
  saveTransactionsToLocalStorage();
};

const toast = useToast();

const handleTransactionDeleted = (index) => {
  transactions.value.splice(index, 1);
  toast.success("Transaction deleted.");
  saveTransactionsToLocalStorage();
};

const saveTransactionsToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :incomes="incomes" :expenses="expenses" />
    <TransactionList
      :transactions="transactions"
      @transactionDeleted="handleTransactionDeleted"
    />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>
