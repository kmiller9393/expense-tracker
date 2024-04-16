<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <TransactionList
      :transactions="transactions"
      @transactionDeleted="handleDelete"
    />
    <AddTransaction @transactionSubmitted="handleSubmit" />
  </div>
</template>

<script setup>
import Balance from "./components/Balance.vue";
import Header from "./components/Header.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

import { useToast } from "vue-toastification";

import { computed, ref, onMounted } from "vue";

const toast = useToast();

const transactions = ref([]);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transactions"));

  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

const total = computed(() => {
  return transactions.value.reduce((acc, cv) => {
    return acc + cv.amount;
  }, 0);
});

// get income
const income = computed(() => {
  return transactions.value
    .filter((t) => t.amount > 0)
    .reduce((acc, cv) => {
      return acc + cv.amount;
    }, 0)
    .toFixed(2);
});

// get expenses
const expenses = computed(() => {
  return transactions.value
    .filter((t) => t.amount < 0)
    .reduce((acc, cv) => {
      return acc + cv.amount;
    }, 0)
    .toFixed(2);
});

// delete transaction
const handleDelete = (id) => {
  transactions.value = transactions.value.filter((t) => t.id !== id);

  saveTransactionsToLocalStorage();

  toast.success("Transaction deleted");
};

// add transaction
const handleSubmit = ({ amount, text }) => {
  transactions.value.push({
    id: generateUniqueId(),
    amount,
    text,
  });

  saveTransactionsToLocalStorage();

  toast.success("Transaction added");
};

// generateUniqueId
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
};

// save to localStorage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};
</script>
