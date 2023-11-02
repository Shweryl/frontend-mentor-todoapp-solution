<template>
  <todo-list/>
</template>

<script>
import TodoList from './components/TodoList.vue'

export default {
  name: 'App',
  components: {
    TodoList
  }
}
</script>

<style lang="scss">
*{
  padding : 0;
  margin : 0;
  box-sizing: border-box;
  font-family: 'Josefin Sans', sans-serif;
}
</style>
