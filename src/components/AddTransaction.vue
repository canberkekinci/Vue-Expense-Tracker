<script setup>
import { reactive, defineEmits } from "vue";
import { useToast } from "vue-toastification";

const form = reactive({
  text: "",
  amount: "",
});

const emit = defineEmits(['transactionSubmitted']);

const toast = useToast();

const handleSubmit = () => {
  if (!form.text || !form.amount) {
    toast.error("Both fields must be filled.");
    return;
  } else {
    emit('transactionSubmitted', form);
    toast.success("Transaction added successfully.");
    form.text = "";
    form.amount = "";
  }
};
</script>

<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="handleSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input v-model="form.text" type="text" id="text" />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input v-model="form.amount" type="text" id="amount" />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>
