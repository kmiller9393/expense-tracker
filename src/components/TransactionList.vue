<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <!-- <li class="minus">
      Cash <span>-$400</span><button class="delete-btn">x</button>
    </li>
    <li class="plus">
      Paycheck <span>$500</span><button class="delete-btn">x</button>
    </li> -->
    <li
      :key="transaction.id"
      v-for="transaction in transactions"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }} <span>${{ transaction.amount }}</span
      ><button class="delete-btn">x</button>
    </li>
  </ul>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});
</script>
