
<template>
  <Header /> 
    <div class="container">
      <Balance :total = "+total" />
      <!-- el mas lo convierte a numeros a partir de string -->
      <IncomeExpense :income="+income" :expenses="+ expenses"/> 
      <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
      <AddTransaction 
      @transaction-submitted="handleTransactionSubmitted"/>
      
    </div>
</template>


<!-- Esto es para colocar codigo de javascript ademas con setup ya no es necesario colocar export default -->
<script setup>
  import Header from "./components/Header.vue";
  import Balance from "./components/Balance.vue";
  import IncomeExpense from "./components/IncomeExpense.vue";
  import TransactionList from "./components/TransactionList.vue";
  import AddTransaction from "./components/AddTransaction.vue";

  import {useToast} from 'vue-toastification';

  import {ref, computed, onMounted} from 'vue';

  const toast = useToast();

  const transactions = ref([]);

  onMounted(()=>{
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

    if(savedTransactions){
      transactions.value = savedTransactions;
    }
  })

  // console.log(transactions.value)

  //Get total
  const total = computed(()=>{
    return transactions.value.reduce((acc, transaction)=>{
      return acc + transaction.amount
    }, 0)
  });


  //Get income
  const income = computed(()=>{
    return transactions.value
    .filter((transaction)=>transaction.amount > 0)
    .reduce((acc, transaction)=>{
      return acc + transaction.amount
    }, 0).toFixed(2);
  })

  //Get expense
  const expenses = computed(()=>{
    return transactions.value
    .filter((transaction)=>transaction.amount < 0)
    .reduce((acc, transaction)=>{
      return acc + transaction.amount
    }, 0).toFixed(2);
  })

  const handleTransactionSubmitted = (transactionData) => {
    transactions.value.push({
      id: generateUniqueId(),
      text: transactionData.text,
      amount: transactionData.amount
    })

    saveTransactionsToLocalStorage();

    toast.success('Transaction added successfully')
  }

  //Generate unique id
  const generateUniqueId = () => {
    return Math.floor(Math.random()*1000000)
  }

  //Deleted transaction

  const handleTransactionDeleted = (id) => {
    transactions.value = transactions.value.filter(
      (transaction)=>transaction.id !== id
    )

    saveTransactionsToLocalStorage()
    toast.success('Transaction deleted')
  }


  //Save to localstorage

  const saveTransactionsToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value))
  }



  // export default {
  //   components: {
  //   Header,
  //   Balance,
  //   IncomeExpense,
  //   TransactionList,
  //   AddTransaction
  // },
  // };
</script> 

<!-- Esto es para colocar codigo css y si coloco scoped solo afectara a este componente-->
<style></style> 