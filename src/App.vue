<template>
  <div id="app">
    <TodoApp />
  </div>
</template>

<script>
import TodoApp from './components/TodoApp.vue';

export default {
  name: 'App',
  components: {
    TodoApp
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');

#app {
  font-family: 'Poppins', sans-serif;
  text-align: center;
  color: #2c3e50;
  background-color: #f0f2f5;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

</style>