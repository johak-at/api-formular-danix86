<script setup>
import { onMounted, ref } from 'vue';
import PocketBase from 'pocketbase';

const pb = new PocketBase('https://pb.seiwald.club');

const studentList = ref([]);
const vorname = ref('');
const nachname = ref('');
const wohnort = ref('');
const geburtstag = ref('');
onMounted(async () => {
    getStudents();
})
async function getStudents() {
    const data = await pb.collection('4bhk').getFullList();
    studentList.value = data;
}
async function addStudent() {
    const newStudent = {
        vorname: vorname.value,
        nachname: nachname.value,
        wohnort: wohnort.value,
        geburtstag: geburtstag.value
    }
    await pb.collection('4bhk').create(newStudent);
    vorname.value = '';
    nachname.value = '';
    wohnort.value = '';
    geburtstag.value = '';
    getStudents();
}
async function deleteStudent(id) {
    await pb.collection('4bhk').delete(id);
    getStudents();
}
</script>

<template>

    <h1>Our Students:</h1>
    <input type="text" v-model="vorname" placeholder="firstname">
    <input type="text" v-model="nachname" placeholder="lastname">
    <input type="text" v-model="wohnort" placeholder="wohnort">
    <input type="date" v-model="geburtstag" placeholder="geburtstag">
    <button @click.prevent="addStudent">Add Student</button>
    <ul>
        <li v-for="student in studentList" :key="student.id">
            {{ student.vorname }} {{ student.nachname }} wohnt in {{ student.wohnort }} und ist am
            {{ student.geburtstag.substring(0, 10) }} geboren.
            <button @click="deleteStudent(student.id)">X</button>
        </li>
    </ul>

</template>