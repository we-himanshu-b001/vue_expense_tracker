<template>
  <Header></Header>
  <div class="container">
    <Balance :total="+total"></Balance>
    <IncomeExpense :income="+income" :expenses="+expenses"></IncomeExpense>
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"></TransactionList>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted"></AddTransaction>
  </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpense from './components/IncomeExpense.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import { ref, computed } from 'vue';
import { useToast } from 'vue-toastification';

const toast = useToast();

const transactions=ref([
  {id:1,name:'Flower',amount:-9.99},
  {id:2,name:'Salary',amount:1119.99},
  {id:3,name:'Book',amount:29.99},
  {id:4,name:'Camera',amount:99.99},
]);

const total=computed(()=>{
  return transactions.value.reduce((acc,transaction) => {
    return acc+transaction.amount;
  },0);
});

const income=computed(()=>{
  return transactions.value.filter((transaction)=> transaction.amount > 0).reduce((acc,transaction) => {
    return acc + transaction.amount;
  },0).toFixed(2);
});

const expenses=computed(()=>{
  return transactions.value.filter((transaction)=> transaction.amount < 0).reduce((acc,transaction) => {
    return Math.abs(acc + transaction.amount);
  },0).toFixed(2);
});

const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: Math.floor(Math.random()*100000),
    name:transactionData.text,
    amount:transactionData.amount,
  });

  toast.success('Transaction Added');
};

const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter(
      (transaction) => transaction.id !== id
  );
  toast.success('Transaction Deleted');
}

</script>