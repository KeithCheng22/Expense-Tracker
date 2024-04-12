<template>
    <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" v-model='text' placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        type="text"
        id="amount"
        v-model="amount"
        placeholder="Enter amount..."
      />
    </div>
    <button class="btn">{{ Object.keys(editedTransaction).length === 0 ? 'Add transaction' : 'Edit Transaction' }}</button>
  </form>
</template>

<script setup>
  import { ref, watchEffect } from 'vue';
  import { useToast } from 'vue-toastification'

  import { generateRandomId } from '../utils'

  const toast = useToast()

  const props = defineProps({
    editedTransaction: {
      type: Object,
      required: false
    }
  })
  const text = ref('');
  const amount = ref('');
  const id = ref('');


  watchEffect(() => {
    const { text: newText, amount: newAmount, id: newId} = props.editedTransaction
    id.value = newId
    text.value =  newText
    amount.value =  newAmount
  })

    const emit = defineEmits([
      'transaction-submitted',
      'reset-edit-transaction'
    ])

    const onSubmit = () => {
      if(!text.value || !amount.value) {
        toast.error('Both fields must be filled');
        return;
      }

      const transactionData = {
        id: id.value || generateRandomId(), 
        text: text.value, 
        amount: parseFloat(amount.value)
      }

      emit('transaction-submitted', transactionData) // payload

      text.value = '';
      amount.value = ''

      // emit('reset-edit-transaction') 

      // setTimeout(() => window.location.href='/', 5500)
    }
</script>