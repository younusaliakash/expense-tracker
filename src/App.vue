<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpense from "./components/IncomeExpensesBoard.vue";
import TransactionHistory from "./components/TransactionHistory.vue";
import AddTransaction from "./components/AddTransaction.vue";
import { computed, onMounted, reactive } from "vue";
import { useToast } from "vue-toastification";

const toast = useToast();
let transactions = reactive([]);

//Retrieves the existing transaction
onMounted(() => {
  const savedData = JSON.parse(localStorage.getItem("transactions"));
  if (savedData) {
    transactions.splice(0, transactions.length, ...savedData);
  }
});

//Total Calculation
const total = computed(() => {
  return transactions
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

//Income calculation
const income = computed(() => {
  return transactions
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2);
});

//Expenses Calculations
const expense = computed(() => {
  return transactions
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0)
    .toFixed(2);
});

// handles transactions
const handleTransaction = (transaction) => {
  transactions.push({ id: getId(), ...transaction });
  saveToLocalStorage();
  toast.success(`"${transaction.text}" has been added`);
};

//get Id from previous transaction
const getId = () => {
  const id =
    transactions.length > 0 ? transactions[transactions.length - 1].id + 1 : 1;
  return id;
};

//Transactions Removed from list
const handleDelete = (id) => {
  transactions.splice(
    transactions.findIndex((transaction) => transaction.id === id),
    1
  );
  saveToLocalStorage();
  toast.success("Transaction Deleted from list");
};

const saveToLocalStorage = () => {
  localStorage.setItem("transactions", JSON.stringify(transactions));
};
</script>

<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpense :income="+income" :expense="+expense" />
    <TransactionHistory
      :transactions="transactions"
      @handleDelete="handleDelete"
    />
    <AddTransaction @handleTransaction="handleTransaction" />
  </div>
</template>
