<script setup>
import { ref } from 'vue';

const emits = defineEmits(['setUserName']);
const userName = ref('');
</script>

<template>
    <h2>Hello Friend! What's your name?</h2>
    <input type="text" v-model="userName" placeholder="Your name" @keyup.enter="emits('setUserName', userName)">
</template>