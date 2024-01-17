<script setup>
import { ref } from 'vue';

const tasks = ref([
    { name: 'Task 1', time: 30 },
    { name: 'Task 2', time: 40 },
    { name: 'Task 3', time: 60 },
    { name: 'Task 4', time: 45 },
    { name: 'Task 5', time: 50 },
]);

// Variables for handling the modal
const editedTask = ref({ name: '', time: 0 });
const editedTaskIndex = ref(null);
const isEditModalVisible = ref(false);

function editTask(index) {
    editedTask.value = { ...tasks.value[index] };
    editedTaskIndex.value = index;
    isEditModalVisible.value = true;
}

function updateTask() {
    if (editedTaskIndex.value !== null) {
        // Update the task in the tasks array
        tasks.value[editedTaskIndex.value] = { ...editedTask.value };

        // Close the modal
        hideModal();
    }
}
function hideModal() {
    $('#myModal').modal('hide');
}
</script>

<template>
    <div class="container mt-5">
        <h1 class="mb-4">Tasks List</h1>

        <ul class="list-group">
            <li v-for="(task, index) in tasks" :key="index"
                class="list-group-item d-flex justify-content-between align-items-center mt-3 mb-3">
                {{ task.name }} - {{ task.time }} minutes
                <button type="button" class="btn btn-info btn-lg ml-5" @click="editTask(index)" data-toggle="modal"
                    data-target="#myModal">Edit</button>
            </li>
        </ul>

        <!-- Modal -->
        <div id="myModal" class="modal fade" role="dialog" v-if="isEditModalVisible">
            <div class="modal-dialog">

                <!-- Modal content-->
                <div class="modal-content">
                    <div class="modal-header">
                        <h4 class="modal-title">Edit Task</h4>
                        <button type="button" class="close" data-dismiss="modal"
                            @click="isEditModalVisible = false">&times;</button>
                    </div>
                    <div class="modal-body text-left">
                        <form @submit.prevent="updateTask">
                            <div class="mb-3">
                                <label for="taskName" class="form-label">Task Name:</label>
                                <input v-model="editedTask.name" type="text" id="taskName" class="form-control" required>
                            </div>

                            <div class="mb-3">
                                <label for="taskTime" class="form-label">Time (minutes):</label>
                                <input v-model="editedTask.time" type="number" id="taskTime" class="form-control" required>
                            </div>

                            <button type="submit" class="btn btn-success">Update Task</button>
                        </form>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-default" data-dismiss="modal"
                            @click="isEditModalVisible = false">Close</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style></style>
