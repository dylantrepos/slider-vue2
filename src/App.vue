<template>
  <div id="app">
    <p>Relation Parent/child test : {{ todos }}</p>
    <TodosApp v-model="todos" @input="updateTodos"></TodosApp>
    <button @click="addTodo">Ajouter une tache</button>
  </div>
</template>

<script>
import TodosApp from './components/TodosApp.vue'

export default {
  name: 'App',
  data() {
    return {
      todos: [{
        name: 'Test de todo',
        completed: false
      }]
    }
  },
  components: {
    TodosApp
  },
  methods: {
    addTodo() {
      this.todos.push({
        name: 'Jeanjean',
        completed: false
      })
    },
    updateTodos(newTodos) {
      this.todos = newTodos;
    }
  }
}
</script>
