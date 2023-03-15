<template>
    <div class="wrapper">
        <form @submit.prevent="submit">
            <input v-model="username" type="text" placeholder="Username" required>
            <input v-model="password" type="password" placeholder="Password" required>
            <button type="submit">Login</button>
            <button @click="closeModal">Cancel</button>
        </form>
    </div>
</template>

<script setup>
import axios from 'axios';
import { ref } from 'vue';

const username = ref('')
const password = ref('')

const emit = defineEmits([
    'close'
])

const closeModal = () => {
    emit('close')
}

const submit = () => {
    axios.post('http://94.74.86.174:8080/api/login', {
        username: username.value,
        password: password.value
    }).then(response => {
        localStorage.setItem('authToken', response.data.data.token)
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