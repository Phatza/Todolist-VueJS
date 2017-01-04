<template lang="html">
  <section class="todoapp">
      <header class="header">
          <h1>Todos</h1>
          <input class="new-todo" placeholder="Ajouter une tache" type="text" v-model="newTodo" @keyup.enter="addTodo">
      </header>
      <div class="main">
        <input type="checkbox" class="toggle-all" v-model="allDone">
          <ul class="todo-list">
              <li class="todo" v-for="todo in filteredTodos" :class="{completed: todo.completed, editing: todo === editing}">
                  <div class="view">
                    <input type="checkbox" class="toggle" v-model="todo.completed">
                      <label @dblclick="editTodo(todo)">{{todo.name}}</label>
                      <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
                  </div>
                  <input type="text" class="edit" v-model="todo.name" @keyup.enter="doneEdit" @keyup.esc="cancelEdit" v-focus="todo === editing" @blur="doneEdit">
              </li>
          </ul>
      </div>
      <footer class="footer" v-show="hasTodos">
        <span class="todo-count"><strong>{{ remaining }}</strong> taches à faire</span>
        <ul class="filters">
          <li><a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'">Toutes</a></li>
          <li><a href="#" :class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'">A faire</a></li>
          <li><a href="#" :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">Faites</a></li>
        </ul>
        <button class="clear-completed" v-show="completed" @click.prevent="deleteCompleted">Supprimer les tâches finies</button>
      </footer>
  </section>
</template>

<script>
export default {
  data() {
    return {
      todos: [],
      newTodo: '',
      filter: 'all',
      editing: null,
      oldTodo: '',
    };
  },
  methods: {
    addTodo() {
      this.todos.push({
        completed: false,
        name: this.newTodo,
      });
      this.newTodo = '';
    },
    deleteTodo(todo) {
      this.todos = this.todos.filter(i => i !== todo);
    },
    deleteCompleted(todo) {
      this.todos = this.todos.filter(todo => !todo.completed);
    },
    editTodo(todo) {
      this.editing = todo;
      this.oldTodo = todo.name;
    },
    doneEdit() {
      this.editing = null;
    },
    cancelEdit() {
      this.editing.name = this.oldTodo;
      this.doneEdit();
    },
  },
  computed: {
    allDone: {
      get() {
        return this.remaining === 0;
      },

      set(value) {
        this.todos.forEach(function (todo) {
          todo.completed = value;
        });
      },
    },

    remaining() {
      return this.todos.filter(todo => !todo.completed).length;
    },
    completed() {
      return this.todos.filter(todo => todo.completed).length;
    },
    filteredTodos() {
      if (this.filter === 'todo') {
        return this.todos.filter(todo => !todo.completed);
      } else if (this.filter === 'done') {
        return this.todos.filter(todo => todo.completed);
      }
      return this.todos;
    },
    hasTodos() {
      return this.todos.length > 0;
    },
  },
  directives: {
    focus(el, value) {
      if (value) {
        el.focus();
      }
    },
  },
};
</script>





















<style src="./todos.css"></style>
