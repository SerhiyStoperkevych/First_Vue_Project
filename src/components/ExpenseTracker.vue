<template>
  <div>
    <h1>Expense Tracker</h1>
    <form @submit.prevent="addExpense">
      <input v-model="newExpense.description" placeholder="Description" />
      <input v-model.number="newExpense.amount" type="number" placeholder="Amount" />
      <button type="submit">Add Expense</button>
    </form>
    <ul>
      <li v-for="expense in expenses" :key="expense.id">
        {{ expense.description }}: ${{ expense.amount }}
        <button @click="removeExpense(expense.id)">Remove</button>
      </li>
    </ul>
    <h2>Total: ${{ totalAmount }}</h2> <!-- Add this line -->
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const expenses = ref([]);
const newExpense = ref({ description: '', amount: '' });

const addExpense = () => {
  if (newExpense.value.description && newExpense.value.amount) {
    expenses.value.push({
      id: Date.now(),
      description: newExpense.value.description,
      amount: parseFloat(newExpense.value.amount),
    });
    newExpense.value = { description: '', amount: '' };
  }
};

const removeExpense = (id) => {
  expenses.value = expenses.value.filter(expense => expense.id !== id);
};

const totalAmount = computed(() => {
  return expenses.value.reduce((total, expense) => total + expense.amount, 0);
});
</script>