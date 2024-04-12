<template>
     <h3>History</h3>
        <ul id="list" class="list">
            <li v-for="transaction in transactions" :key='transaction.id' :class="transaction.amount < 0 ? 'minus' : 'plus'">
                {{ transaction.text }} 
                <div class="edit-btn-amount">
                    <span>${{ transaction.amount }}</span>
                    <button class="edit-btn" @click="editTransaction(transaction.id)">Edit</button>
                </div>
                <button class="delete-btn" @click="deleteTransaction(transaction.id)" >x</button>
            </li>
        </ul>
</template>

<script setup>
    defineProps({
        transactions: {
            type: Array,
            required: true
        }
    })
    const emit = defineEmits([
        'delete-transaction',
        'edit-transaction'
    ])
    const deleteTransaction = (transaction_id) => {
        emit('delete-transaction', transaction_id) // Payload
    }

    const editTransaction = (transaction_id) => {
        emit('edit-transaction', transaction_id)
    }
</script>
