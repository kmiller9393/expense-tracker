<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses />
    <TransactionList :transactions="transactions" />
    <AddTransaction />
  </div>
</template>

<script setup>
import Balance from "./components/Balance.vue";
import Header from "./components/Header.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

import { computed, ref } from "vue";

const transactions = ref([
  {
    id: 1,
    text: "Flower",
    amount: -19.99,
  },
  {
    id: 2,
    text: "Salary",
    amount: 299.97,
  },
  {
    id: 3,
    text: "Book",
    amount: -10,
  },
  {
    id: 4,
    text: "Camera",
    amount: 150,
  },
]);

const total = computed(() => {
  return transactions.value.reduce((acc, cv) => {
    return acc + cv.amount;
  }, 0);
});

// get income

// get expenses
</script>
