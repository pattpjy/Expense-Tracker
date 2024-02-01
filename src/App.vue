<template>
  <GreetingModal v-if="modalStatus" @closedModal="handleClosedModal" />
  <div class="container" v-if="!modalStatus">
    <Header />
    <Balance :total="+total" /><IncomeExpense
      :income="+income"
      :expense="+expense"
    /><TransactionList
      :transactions="transactions"
      @transactionDeleted="handleTransactionDeleted"
    /><AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>
<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import GreetingModal from './components/GreetingModal.vue';
import { ref, computed, onMounted } from 'vue';
import { useToast } from 'vue-toastification';

const toast = useToast();

const transactions = ref([]);
onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});
// total
const total = computed(() => {
  return transactions.value.reduce((acc, transactions) => {
    return acc + transactions.amount;
  }, 0);
});

// income
const income = computed(() => {
  return transactions.value
    .filter((tran) => tran.amount > 0)
    .reduce((acc, transactions) => {
      return acc + transactions.amount;
    }, 0)
    .toFixed(2);
});

// expense

const expense = computed(() => {
  return transactions.value
    .filter((tran) => tran.amount < 0)
    .reduce((acc, transactions) => {
      return acc + transactions.amount;
    }, 0)
    .toFixed(2);
});

// Add transaction
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount,
  });
  savedTransactionsToLocalStorage();
  toast.success('Transaction added');
};

// Generate ID
const generateUniqueId = () => {
  return Math.floor(Math.random() * 1000000);
};
//delete transaction

const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter((tran) => tran.id !== id);
  savedTransactionsToLocalStorage();
  toast.success('Transaction deleted');
};

//Save to local storage
const savedTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
};

//Greeting Trigger
const modalStatus = ref(true);

//Greeting Handle Close button clicked

const handleClosedModal = () => {
  modalStatus.value = false;
  console.log('click');
};
</script>
