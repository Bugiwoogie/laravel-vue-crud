<template>
  <div>
    <h1>Task Management</h1>
    <TaskList />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import TaskList from '../Components/TaskList.vue';

export default defineComponent({
  components: {
    TaskList,
  },
});
</script>
