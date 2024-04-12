<template>
    <Header />
    <div class="container">
        <Balance :total='+total'/>
        <IncomeExpenses :income="income" :expenses="expenses"/>
        <TransactionList :transactions="transactions" @delete-transaction="deleteTransaction" @edit-transaction="editTransaction"/>
        <AddTransaction @transaction-submitted="handleTransactionSubmitted" :editedTransaction="editedTransaction"/>
    </div>
</template>

<script setup>
    // Component Imports
    import Header from './components/Header.vue';
    import Balance from './components/Balance.vue';
    import IncomeExpenses from './components/IncomeExpenses.vue';
    import TransactionList from './components/TransactionList.vue';
    import AddTransaction from './components/AddTransaction.vue';
    // Vue Imports
    import { ref, computed, onMounted, watch } from 'vue';

    // Third-Party Imports
    import { useToast } from 'vue-toastification'


    const toast = useToast()
    
    onMounted(() => {
        const savedTransactions = JSON.parse(localStorage.getItem('transactions'))

        if (savedTransactions) {
            transactions.value = savedTransactions
        }
    })

    const transactions = ref([]);
    const editedTransaction = ref({})

    
    // Get Total
    const total = computed(() => {
        return transactions.value.reduce((acc, transaction) => {
            return acc + transaction.amount;
        }, 0)
    })

    // Get income
    const income = computed(() => {
        return + transactions.value
        .filter(transaction => transaction.amount > 0)
        .reduce((acc, transaction) => {
            return acc + transaction.amount;
        }, 0).toFixed(2)
    })

    // Get expenses
    const expenses = computed(() => {
        return + transactions.value
        .filter(transaction => transaction.amount < 0)
        .reduce((acc, transaction) => {
            return acc + transaction.amount;
        }, 0).toFixed(2)
    })

    const handleTransactionSubmitted = (newTransaction) => {
        const { text, amount, id } = newTransaction
        const found = transactions.value.find(transaction => transaction.id === +id)

        if (found) {
            found.text = text
            found.amount = amount
            toast.success('Transaction edited');
        } else {
            transactions.value.push({
            id,
            text,
            amount
        })
        toast.success('Transaction added');
        }
        editedTransaction.value = {}

        savedTransactionsToLocalStorage()

    }

    const deleteTransaction = (transaction_id) => {
        transactions.value = transactions.value.filter(transaction => transaction.id !== transaction_id)
        savedTransactionsToLocalStorage()
        toast.success("Transaction Deleted")
    }

    const editTransaction = (transaction_id) => {
        // Get the transaction to be updated
        editedTransaction.value = transactions.value.filter(transaction => transaction.id === transaction_id)[0]
    }

    // Save to localStorage
    const savedTransactionsToLocalStorage = () => {
        localStorage.setItem('transactions', JSON.stringify(transactions.value))
    }


</script>