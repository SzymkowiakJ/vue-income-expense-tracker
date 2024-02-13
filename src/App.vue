<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import {ref, computed} from 'vue'
import {v4 as uuid} from 'uuid'
const transactions = ref([
  { id: uuid(), text: 'Flower', amount: -18.99 },
  { id: uuid(), text: 'Salary', amount: 299.5 },
  { id: uuid(), text: 'Book', amount: -10 },
  { id: uuid(), text: 'Camera', amount: 150 }
]);

const total = computed(()=>{
  return transactions.value.reduce((acc, transaction)=>{return acc+ transaction.amount}, 0)
})
const income = computed(()=>{
  return transactions.value.reduce((acc, transaction)=>{
    return (transaction.amount>=0? acc+transaction.amount : acc)
  }, 0).toFixed(2);
})
const expenses = computed(()=>{
  return transactions.value.reduce((acc, transaction)=>{
    return (transaction.amount<0? acc+transaction.amount : acc)
  }, 0).toFixed(2)
})

const handleTransactionSubmitted = (transactionData)=>{
  transactions.value.push({id: uuid(), text: transactionData.text, amount: transactionData.amount})
  console.log(transactions.value)
}

const handleDeleteTransaction = (transactionId)=>{
  transactions.value = transactions.value.filter(transaction => transaction.id !== transactionId)
}
</script>

<template>
  <Header />
  <div class="container">
    <Balance :balance="total"/>
    <IncomeExpenses :income="+income" :expenses="+expenses"/>
    <TransactionList v-bind:transactions="transactions" @deleteTransaction="handleDeleteTransaction"/>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
  </div>
</template>