<template>
  <div>
    <h2>Tasks</h2>
    <ul>
      <li v-for="task in tasks" :key="task.id">
        {{ task.title }} - {{ task.completed ? 'Completed' : 'Pending' }}
        <button @click="editTask(task)">Edit</button>
        <button @click="deleteTask(task.id)">Delete</button>
      </li>
    </ul>
    <form @submit.prevent="addTask">
      <input v-model="newTask.title" placeholder="Task title" required>
      <textarea v-model="newTask.description" placeholder="Description"></textarea>
      <button type="submit">Add Task</button>
    </form>
  </div>
</template>

<script lang="ts">
  import { defineComponent, ref, onMounted } from 'vue';
  import axios from 'axios';

  interface Task {
  id: number;
  title: string;
  description: string | null;
  completed: boolean;
  }

  export default defineComponent({
  setup() {
    const tasks = ref<Task[]>([]);
    const newTask = ref<Partial<Task>>({ title: '', description: '', completed: false });

    const fetchTasks = async () => {
      const response = await axios.get('/api/tasks');
      tasks.value = response.data;
    };

    const addTask = async () => {
      await axios.post('/api/tasks', newTask.value);
      newTask.value = { title: '', description: '', completed: false };
      await fetchTasks();
    };

    const editTask = async (task: Task) => {
      const updatedTask = { ...task, completed: !task.completed };
      await axios.put(`/api/tasks/${task.id}`, updatedTask);
      await fetchTasks();
    };

    const deleteTask = async (id: number) => {
      await axios.delete(`/api/tasks/${id}`);
      await fetchTasks();
    };

    onMounted(fetchTasks);

    return {
      tasks,
      newTask,
      addTask,
      editTask,
      deleteTask,
    };
  },
  });
</script>