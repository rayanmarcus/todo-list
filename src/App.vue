<template>
  <MyTodo/>
</template>

<script>
import MyTodo from './components/MyTodo.vue'


export default {
  name: 'App',
  components: {
    MyTodo
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
