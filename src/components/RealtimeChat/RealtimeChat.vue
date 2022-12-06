<script setup>
import NameInput from './NameInput.vue';
import { ref } from 'vue';

const userName = ref('');
function setUserName(name) {
    userName.value = name;
}
</script>

<template>
    <NameInput @set-user-name="setUserName" v-if="!userName"></NameInput>
    <div v-else>
        <h2>Hi {{ userName }}!</h2>
    </div>

</template>