<script setup>
import { ref } from 'vue';
const tasks = ref([])
const inputValue = ref('')
const editedTaskName = ref('')
let nextId = 1

const pushTask = () => {
    const newTask = {
        id: nextId,
        name: inputValue.value,
        editing: false
    }
    nextId++
    tasks.value.push(newTask)
    inputValue.value = ''
}

const deleteTasks = (index) => {
    if (index > -1) {
        confirm('Sei sicuro di eliminare la task?')
        tasks.value.splice(index, 1)
    }
}

const startEditing = (index) => {
    tasks.value[index].editing = true;
    editedTaskName.value = tasks.value[index].name;
}

const saveTask = (index) => {
    tasks.value[index].name = editedTaskName.value;
    tasks.value[index].editing = false;
}

</script>

<template>
    <div class="container">
        <div class="row">
            <div class="col d-flex justify-content-center align-items-center vh-100">
                <div class="border h-50 w-50 text-center p-5 rounded-5 bg-info">
                    <h2 for="title mb-2">To-do list</h2>
                    <form @submit.prevent="pushTask">
                    <input class="form-control" type="text" v-model="inputValue" >
                    <button class="btn btn-light mt-2"  v-if="inputValue.length >= 1">Invia</button>
                    </form>
                    <div class="d-flex aling-items-center justify-content-around mt-3 ">
                        <div class="table-responsive" style="max-height: 200px;">
                        <table class="table">
                            <thead>
                                <tr>
                                    <th>Indice</th>
                                    <th>Nome task</th>
                                    <th>Azioni</th>
                                </tr>
                            </thead>
                            <tbody>
                                <template v-for="(task, index) in tasks" :key="task.id">
                                    <tr v-if="!task.editing">
                                        <td>{{ index + 1}}</td>
                                        <td>{{ task.name }}</td>
                                        <td>
                                            <button class="btn btn-danger" @click="deleteTasks(index)">Elimina</button>
                                            <button class="btn btn-warning ms-2" @click="startEditing(index)">Modifica</button>
                                        </td>
                                    </tr>
                                    <tr v-else>
                                        <td>{{ task.id }}</td>
                                        <td>
                                            <input v-model="editedTaskName" type="text">
                                        </td>
                                        <td>
                                            <button class="btn btn-success" @click="saveTask(index)">Salva</button>
                                        </td>
                                    </tr>
                                </template>
                            </tbody>
                        </table>
                    </div>
                    </div>
                </div>

            </div>
        </div>

    </div>
</template>

