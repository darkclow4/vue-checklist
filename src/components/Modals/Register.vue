<template>
    <div class="wrapper">
        <form @submit.prevent="submit">
            <input v-model="email" type="email" placeholder="Email" required>
            <input v-model="username" type="text" placeholder="Username" required>
            <input v-model="password" type="password" placeholder="Password" required>
            <button type="submit">Register</button>
            <button @click="closeModal">Cancel</button>
        </form>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';

const email = ref('')
const username = ref('')
const password = ref('')

const emit = defineEmits([
    'close'
])

const closeModal = () => {
    emit('close')
}

const submit = () => {
    axios.post('http://94.74.86.174:8080/api/register', {
        email: email.value,
        username: username.value,
        password: password.value
    }).then(response => {
        console.log(response)
    }).catch(e => {
        console.log(e)
    })
    emit('close')
}
</script>

<style scoped>
.wrapper {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
}

form {
    display: flex;
    flex-direction: column;
}
</style>