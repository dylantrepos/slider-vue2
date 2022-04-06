<template>
    <section class="todoapp">
        <header class="header">
            <h1>Todos</h1>
            <input type="text" class="new-todo" placeholder="Ajouter une tâche" v-model="newTodo" @keyup.enter="addTodo">
        </header>
        <div class="main">
            <input id="toggle-all" type="checkbox" class="toggle-all" v-model="allDone">
            <label for="toggle-all">Tout completer</label>

            <ul class="todo-list">
                <li class="todo" v-for="(todo, index) in showTasks" :key="index" :class="{completed: todo.completed, editing: todo === editing}">
                    <div class="view">
                        <input type="checkbox" v-model="todo.completed" class="toggle">
                        <label @dblclick="editTodo(todo)">{{ todo.name }}</label>
                        <button class="destroy" @click="deleteTodo(index)"></button>
                    </div>
                    <input type="text" class="edit" 
                        v-model="todo.name" 
                        @keyup.enter="doneEdit" 
                        @blur="doneEdit" 
                        @keyup.esc="cancelEdit"
                        v-focus="todo === editing"
                        >
                </li>
            </ul>
        </div> 
        <footer class="footer" v-show="hasTodo">
            <span class="todo-count"><strong>{{remaining}}</strong> tâche(s) à faire</span>
            <ul class="filters">
                <li><a href="#" :class="{selected: filter == 'all'}" @click.prevent="filter = 'all'">Toutes</a></li>
                <li><a href="#" :class="{selected: filter == 'todo'}" @click.prevent="filter = 'todo'">À faire</a></li>
                <li><a href="#" :class="{selected: filter == 'done'}" @click.prevent="filter = 'done'">Faites</a></li>
            </ul>
            <button class="clear-completed" @click="removeTasksDone()" v-show="doneTodo > 0">Supprimer les tâches finies</button>
        </footer>
    </section>
</template>

<script>

import Vue from 'vue';

export default {
    props: {
        value: {
            type: Array,
            default() {
                return []
            }
        }
    },
    data() {
        return {
            todos: this.value,
            newTodo: '',
            filter: 'all',
            editing: null,
            oldTodo: ''
        }
    },

    methods: {
        addTodo() {
            if(!this.newTodo.trim()) return;
            this.todos.push({
                name: this.newTodo,
                completed: false
            });
            this.newTodo = '';
        },
        deleteTodo(index) {
            if(this.todos[index]) this.todos.splice(index, 1);
            this.$emit('input', this.todos);
        },
        removeTasksDone() {
            this.todos = this.todos.filter(todo => !todo.completed);
            this.$emit('input', this.todos);
        },
        editTodo(todo) {
            this.editing = todo;
            this.oldTodo = todo.name

        },
        doneEdit() {
            this.editing = null;
            this.oldTodo = '';
        },
        cancelEdit() {
            this.editing.name = this.oldTodo;
            this.doneEdit();
        }
    },

    computed: {
        remaining() {
            const tasks = this.todos.filter(task => !task.completed);
            return tasks.length;
        },
        showTasks() {
            if(this.filter == 'todo') return this.todos.filter(task => !task.completed);
            if(this.filter == 'done') return this.todos.filter(task => task.completed);
            return this.todos;
        },
        allDone: {
            get () {
                return this.remaining === 0;
            },
            set (value) {
                this.todos.forEach(todo => todo.completed = value)
            }
        },
        hasTodo() {
            return this.todos.length > 0;
        },
        doneTodo() {
            return this.todos.filter(todo => todo.completed).length
        }
    },

    directives: {
        /*
        * Récupère l'élément HTML pour accès en JS.
        * La fonction nextTick() permet de retourner le résultat 
        * lors du prochain chargement du DOM.
        */
        focus(el, value) {
            Vue.nextTick(() => {if(value) el.focus()})
        }
    }

}
</script>

<style src="../../node_modules/todomvc-app-css/index.css">

</style>