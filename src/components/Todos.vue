<template lang="html">
  <section class="todoapp">
      <header class="header">
          <h1>Todos</h1>
          <input class="new-todo" placeholder="Ajouter une tache" type="text" v-model="newTodo" @keyup.enter="addTodo">
      </header>
      <div class="main">
        <input type="checkbox" class="toggle-all" v-model="allDone">
          <ul class="todo-list">
              <li class="todo" v-for="todo in filteredTodos" :class="{completed: todo.completed}">
                  <div class="view">
                    <input type="checkbox" class="toggle" v-model="todo.completed">
                      <label>{{todo.name}}</label>
                  </div>
              </li>
          </ul>
      </div>
      <footer class="footer">
        <span class="todo-count"><strong>{{ remaining }}</strong> taches à faire</span>
        <ul class="filters">
          <li><a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter = 'all'">Toutes</a></li>
          <li><a href="#" :class="{selected: filter === 'todo'}" @click.prevent="filter = 'todo'">A faire</a></li>
          <li><a href="#" :class="{selected: filter === 'done'}" @click.prevent="filter = 'done'">Faites</a></li>
        </ul>
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
  },
  computed: {
    allDone: {
      get() {

      },
      set(value) {
        this.todos.forEach((todo) => {
          var todo.completed = value;
        });
      },
    },
    remaining() {
      return this.todos.filter(todo => !todo.completed).length;
    },
    filteredTodos() {
      if (this.filter === 'todo') {
        return this.todos.filter(todo => !todo.completed);
      } else if (this.filter === 'done') {
        return this.todos.filter(todo => todo.completed);
      }
      return this.todos;
    },
  },
};
</script>





















<style src="./todos.css"></style>
