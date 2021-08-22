<template>
  <div id="app">
    <NewTask />
    <ToDoList></ToDoList>
  </div>
</template>

<script>
import NewTask from './components/NewTask.vue';
import ToDoList from './components/ToDoList.vue';

export default {
  name: 'App',
  components: {
    NewTask,
    ToDoList
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  display:grid;
  grid-template-columns: 1fr 1fr;
}

h5 {
  text-align: center;
}

textarea {
  width: 80%;
  margin: auto;
}
</style>
