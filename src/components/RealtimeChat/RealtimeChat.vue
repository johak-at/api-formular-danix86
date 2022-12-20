<script setup>
import { ref, onMounted } from 'vue';
import NameInput from './NameInput.vue';
import pb from '@/database/pb.js';
const userName = ref();
function setUserName(name) {
    userName.value = name;
}
const chatMessages = ref([]);
const newMessage = ref('');
async function postMessage() {
    const record = await pb.collection('rtc_4bhk').create({
        sender: userName.value,
        message: newMessage.value,
    });
    newMessage.value = '';
}
async function loadChatMessages() {
    const data = await pb.collection('rtc_4bhk').getFullList();
    chatMessages.value = data.reverse();
}
onMounted(() => {
    loadChatMessages();
    pb.collection('rtc_4bhk').subscribe('*', function (e) {
        loadChatMessages();
    });
});
</script>

<template>
    <NameInput @set-user-name="setUserName" v-if="!userName"></NameInput>
    <div v-else>
        <h2>Hi {{ userName }}!</h2>
        <input type="text" placeholder="type your message" v-model="newMessage" @keyup.enter="postMessage" />
        <ul>
            <li class="msg" :class="{ self: msg.sender === userName }" v-for="msg in chatMessages" :key="msg.id">
                <div style="fontSize: 0.7rem">{{ msg.sender }}:</div>
                <div>{{ msg.message }}</div>
            </li>
        </ul>
    </div>
</template>

<style scoped>
.msg {
    width: 300px;
    display: flex;
    flex-direction: column;
    text-align: left;
    list-style: none;
    line-height: 1rem;
    margin-bottom: 0.5rem;
}

.self {
    text-align: right;
}
</style>