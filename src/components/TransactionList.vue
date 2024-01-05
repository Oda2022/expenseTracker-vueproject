<template>
    <h3> History </h3>
        <ul id="list" class="list">
            <li v-for="transaction in transactions"
                v-bind:key="transaction.id" 
                    :class="transaction.amount < 0 ? 'minus' : 'plus'"
            >
                {{ transaction.text }} <span>${{ transaction.amount }}</span><button 
                @click="deleteTransaction(transaction.id)" class="delete-btn"> x </button>
            
            </li>
            <!-- <li class="minus">
                Cash <span>-$400</span><button
                class="delete-btn">x</button>
            </li>

            <li class="plus">
                Paycheck <span>$800</span><button
                class="delete-btn">x</button>
            </li> -->
        </ul>

</template>

<script setup>
    // import {defineProps} from 'vue'; //ya no se importa, solo se usa
    const emit = defineEmits(['transactionDeleted'])

    const props = defineProps({
        transactions: {
            type: Array,
            required: true
        }
    });

    const deleteTransaction = (id) => {
        emit('transactionDeleted', id)
    }

</script>





// <!-- <script setup>

// const transactions = [
//     {id:1, text:'flower', amount:-19.99},
//     {id:2, text:'salary', amount:299.97},
//     {id:3, text:'book', amount:-10},
//     {id:4, text:'camera', amount:150},
// ] -->
// <!-- // export default {

    // setup() {
    //     const transactions = [
    //         {id:1, text:'flower', amount:-19.99},
    //         {id:2, text:'salary', amount:299.97},
    //         {id:3, text:'book', amount:-10},
    //         {id:4, text:'camera', amount:150},
    //     ]

    //     return{
    //         transactions,
    //     }
    // }
    // data(){
    //     return {
    //         transactions: [
    //             {id:1, text:'flower', amount:-19.99},
    //             {id:2, text:'salary', amount:299.97},
    //             {id:3, text:'book', amount:-10},
    //             {id:4, text:'camera', amount:150},
    //         ]
    //     }
    // }
// } -->
// <!-- </script> -->