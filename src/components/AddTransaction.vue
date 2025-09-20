<script setup>
import { reactive, defineProps } from "vue";

const form = reactive({
  text: "",
  amount: "",
});

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

const handleSubmit = () => {
  props.transactions.push({
    id: props.transactions.slice(-1)[0].id + 1,
    text: form.text,
    amount: Number(form.amount),
  });
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
