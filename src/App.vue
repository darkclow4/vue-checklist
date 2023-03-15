<script setup>
import { onMounted, ref } from "vue";
import Checklist from "./components/Checklist.vue";
import Header from "./components/Header.vue";

const authToken = ref(null);
const login = () => {
  authToken.value = localStorage.getItem('authToken')
}
onMounted(()=>{
  authToken.value = localStorage.getItem('authToken')
})
</script>

<template>
  <div>
    <header>
      <Header v-if="!authToken" @login="login" />
    </header>
    <main>
      <Checklist v-if="authToken" />
    </main>
  </div>
</template>

<style scoped>
</style>
