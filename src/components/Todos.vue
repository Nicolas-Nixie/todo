<template>
    <section class="todoapp">
        <header class="header">

            <h1>Todos</h1>
            <span class="todo-count"><strong>{{ completed }}</strong> tâches faites</span>
                <span class="todo-count"><strong>{{ inProgress }}</strong> tâches en cours</span>
            <input type="test" class="new-todo"  placeholder="Ajouter une tache" v-model="newTodo" @keyup.enter="addTodo">

        </header>

        <div class="main">
            <input type="checkbox" class="toggle-all" v-model="allDone">
            <ul class="todo-list">
                <li class="todo" v-for="(todo, index) in filteredTodos" :key="index" :class="{completed: todo.completed, editing: todo === editing}">
                    <div class="view">
                        <input type="checkbox" v-model="todo.completed" class="toggle" :disabled="verification(todo.name)">
                        <label @dblclick="editTodo(todo)" >{{ todo.name }}</label>
                        <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
                        <p v-show="todo.inProgress">encour</p>
                    </div>
                    <input type="text" class="edit" v-model="todo.name" @keyup.enter="doneEdit" @blur="doneEdit" @keyup.esc="cancelEdit" v-focus="todo === editing">
                <input type="checkbox" v-model="todo.inProgress">
                </li>
            </ul>
        </div>
        <footer class="footer" v-show="hasTodos">
            <span class="todo-count"><strong>{{ remaining }}</strong> tâches à faire</span>
            <ul class="filters">
                <li><a href="#" class="{selected: filter === 'all'}" @click.prevent="filter = 'all'">Toutes</a></li>
                <li><a href="#" class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'">A faire</a></li>
                <li><a href="#" class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">Faites</a></li>
            </ul>
            <button class="clear-completed" v-show="completed" @click.prevent="deleteCompleted">Supprimer les tâhces finis</button>
        </footer>
    </section>
</template>

<script>

import Vue from 'vue'

export default {
  props: {
    value: {type: Array, default () { return [] }}
  },

  data () {
    return {
      todos: this.value,
      newTodo: '',
      filter: 'all',
      editing: null,
      oldTodo: ''
    }
  },

  watcher: {
    value (value) {
      this.todos = value
    }
  },

  methods: {
    addTodo () {
      this.todos.push({
        completed: false,
        name: this.newTodo,
        inProgress: false
      })

      this.newTodo = ''
    },

    deleteTodo (todo) {
      this.todos = this.todos.filter(i => i !== todo)
      this.$emit('input', this.todos)
    },

    deleteCompleted () {
      this.todos = this.todos.filter(todo => !todo.completed)
      this.$emit('input', this.todos)
    },

    editTodo (todo) {
      this.editing = todo
      this.oldTodo = todo.name
    },

    doneEdit () {
      this.editing = null
    },

    cancelEdit () {
      this.editing.name = this.oldTodo
      this.doneEdit()
    },

    verification (name) {
      const todo = this.todos.find(todo => todo.name === name)
      return !todo.inProgress
    }
  },

  computed: {
    allDone: {
      get () {
        return this.remaining === 0
      },
      set (value) {
        this.todos.forEach(todo => {
          todo.completed = value
          todo.verification = value
        })
      }
    },

    remaining () {
      return this.todos.filter(todo => !todo.completed).length
    },

    completed () {
      const test = this.todos.filter(function (todo) {
        return todo.completed
      })
      return test.length
    },

    hasTodos () {
      return this.todos.length > 0
    },

    filteredTodos () {
      if (this.filter === 'todo') {
        return this.todos.filter(todo => !todo.completed)
      } else if (this.filter === 'done') {
        return this.todos.filter(todo => todo.completed)
      }

      return this.todos
    },

    inProgress () {
      return this.todos.filter(todo => todo.inProgress).length
    }
  },

  directives: {
    focus (el, value) {
      if (value) {
        Vue.nextTick(_ => {
          el.focus()
        })
      }
    }
  }

}

</script>

<style src="./todos.css"></style>
