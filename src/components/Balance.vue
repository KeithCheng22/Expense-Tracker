<template>
    <h4>Your Balance</h4>
    <h1 id="balance" class='money' :class="total >= 0 ? 'plus' : 'money minus'">${{ total }}</h1>
</template>

<script setup>
    import { defineProps } from 'vue';
    
    const props = defineProps({
        total: {
            type: Number,
            required: true
        }
    })
</script>