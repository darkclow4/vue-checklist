<template>
    <div>
        <Login v-if="loginModal" @close="emitLogin" />
        <Register v-if="registerModal" @close="registerModal = false" />
        <div>
            <div @click="showLogin">Login</div>
            <div @click="showRegister">Register</div>
        </div>
    </div>
</template>

<script setup>
import { ref,computed } from 'vue';
import Login from './Modals/Login.vue'
import Register from './Modals/Register.vue'

const loginModal = ref(false);
const registerModal = ref(false);
const emit = defineEmits(['login'])

const showLogin = () => {
    loginModal.value = true
}
const showRegister = () => {
    registerModal.value = true
}

const emitLogin = () => {
    emit('login')
    loginModal.value = false
}
</script>

<style scoped></style>