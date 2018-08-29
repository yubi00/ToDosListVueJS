<template>
  <div id="app">
    <HelloWorld msg="To-Dos List"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import UpdateTodo from './components/UpdateTodo.vue'
import FilterTodo from './components/FilterTodo.vue'

export default {
  name: 'app',
  components: {
    HelloWorld,UpdateTodo,FilterTodo
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
