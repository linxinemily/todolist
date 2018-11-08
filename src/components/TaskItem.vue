<template>
    <div class="item mb-3">
    <!-- normal -->
    <div class="card" v-if="editingItem !== item">
        <div class="card-body" >
        <div class="float-left">
            <input @change="toggleTask(item)" type="checkbox" :checked="item.completed">
            <span>{{ item.title }}</span>
        </div>
        <button @click="editTask(item)" class="btn btn-primary float-right">Edit</button>
        <button @click="deleteTask(item)" class="btn btn-danger float-right">Delete</button>
        </div>
    </div>
    <!-- end normal -->

    <!-- editing -->
    <task-form v-else
        class="mb-3"
        :item="item"
        @submit="updateTask"
        @cancel="cancelEditing"
        submitText="Update Task">
    </task-form>
    <!-- end editing -->
    </div>
</template>

<script>
import TaskForm from './TaskForm.vue'

export default {
    components: {
        TaskForm
    },
    props: ['item', 'editingItem', 'editTask', 'updateTask', 'cancelEditing', 'deleteTask', 'toggleTask']
}
</script>
