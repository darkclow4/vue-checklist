<template>
    <div>
    <div>
        <input v-model="checklistNew" type="text" placeholder="New Checklist">
        <button @click="addChecklist">Create Checklist</button>
        
    </div>
    <div>
        <table>
            <thead>
                <tr>
                    <td>Name</td>
                    <td>Items</td>
                    <td>Status</td>
                    <td>Actions</td>
                </tr>
            </thead>
            <tbody>
                <tr v-for="data,index in checklist" :id="index">
                    <td>{{ data.name }}</td>
                    <td>{{ data.items }}</td>
                    <td>{{ data.checklistCompletionStatus }}</td>
                    <td>
                        <button @click="deleteChecklist(data.id)">Delete</button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</div>
</template>

<script setup>
import axios from 'axios';
import { ref,onMounted } from 'vue';

const authToken = ref('')
const checklist = ref([])
const checklistNew = ref('')

const getChecklist = () => {
    axios.get('http://94.74.86.174:8080/api/checklist', {
        headers: {
            'Authorization': `Bearer ${authToken.value}`
        }
    })
    .then(response => {
        checklist.value = response.data.data
    }).catch(e => {
        alert(e.response.data.errorMessage)
    })
}

const addChecklist = () => {
    if(checklistNew.value == '') {
        alert('Checklist tidak boleh kosong')
        return
    }
    axios.post('http://94.74.86.174:8080/api/checklist', {
        name: checklistNew.value
    }, {
        headers: {
            'Authorization': `Bearer ${authToken.value}`
        }
    }).then((response)=>{
        getChecklist()
        alert(response.data.message)
    })
    .catch(e => {
        if(e.response.data.errorMessage == 'JWT token malformed')
        alert('Silahkan login terlebih dahulu')
    })
    checklistNew.value = ''
}

const deleteChecklist = (id) => {
    axios.delete(`http://94.74.86.174:8080/api/checklist/${id}`, {
        headers: {
            'Authorization': `Bearer ${authToken.value}`
        }
    }).then((response)=>{
        getChecklist()
        alert(response.data.message)
    })
    .catch(e => {
        console.log(e.response.data.errorMessage)
    })
}
onMounted(()=>{
    authToken.value = localStorage.getItem('authToken')
    getChecklist()
})
</script>

<style lang="scss" scoped>

</style>