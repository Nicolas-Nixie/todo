<template>
  <div>
    {{ todos }}
    <todos v-model="todos"></todos>
  </div>

</template>

<script>

import Todos from './components/Todos.vue'

export default {

  data () {
    return {
      todos: [{
        name: 'Demo',
        completed: false,
        inProgress: true
      }]
    }
  },

  methods: {
    addTodo () {
      this.todos.push({
        name: 'Jean',
        completed: false,
        inProgress: false
      })
    }
  },

  components: {
    Todos
  },
  name: 'App'

}
</script>

<style>
/*
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
*/
</style>
