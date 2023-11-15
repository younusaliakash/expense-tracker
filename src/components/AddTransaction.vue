<script setup>
import { reactive } from "vue";
import { useToast } from "vue-toastification";
import { defineEmits } from "vue";
const emit = defineEmits(["handleTransaction"]);

const toast = useToast();

const transaction = reactive({
  text: "",
  amount: "",
});

const handleSubmit = () => {
  if (!transaction.text || !transaction.amount) {
    toast.error("You Must fill-up Both Text and Amount field");
    return;
  }

  emit("handleTransaction", transaction);
  transaction.amount = "";
  transaction.text = "";
};
</script>

<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="handleSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input
        type="text"
        v-model="transaction.text"
        id="text"
        placeholder="Enter text..."
      />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        type="number"
        id="amount"
        v-model="transaction.amount"
        placeholder="Enter amount..."
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
