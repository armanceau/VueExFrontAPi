<script setup>
import HelloWorld from './components/HelloWorld.vue'
import { ref } from 'vue'
import Task from './components/Task.vue'
import AddTask from './components/AddTask.vue';
const items = ref([{ message: 'Foo' }, { message: 'Bar' }])
</script>

<template>
  <h1>todo list</h1>
  <AddTask></AddTask>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
