<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpense from "./components/IncomeExpensesBoard.vue";
import TransactionHistory from "./components/TransactionHistory.vue";
import AddTransaction from "./components/AddTransaction.vue";
</script>

<template>
  <Header />
  <div class="container">
    <Balance/>
    <IncomeExpense/>
    <TransactionHistory/>
    <AddTransaction/>
  </div>
</template>
