<template>
  <div class="greeting">
    <h1>Welcome to Budget Tracker Tool</h1>
    <p>
      This budget tracking tool designed to simplify your financial journey.
      Take control of your money, effortlessly manage your income and expenses,
      and stay on top of your financial goals.
    </p>
    <button class="btn" @click="handleClosedModal">Close</button>
  </div>
</template>
<script setup>
import { defineProps } from 'vue';
const emit = defineEmits(['closedModal']);

const handleClosedModal = () => {
  emit('closedModal');
};
</script>
