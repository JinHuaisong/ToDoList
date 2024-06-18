<template>
  <div id="app">
    <ToDoList />
  </div>
</template>

<script>
import ToDoList from './components/ToDoList.vue';

export default {
  name: 'App',
  components: {
    ToDoList
  }
};
</script>

<style>
body {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  background-color: #eaeaea;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}
</style>
