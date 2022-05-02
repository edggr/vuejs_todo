<template>
  <div>
    <todo @delete-todo="deleteTodo" @save-todos="saveTodos" @complete-todo="completeTodo" @uncomplete-todo="uncompleteTodo" v-for="todo in todos" :todo.sync="todo" :key="todo.id"></todo>
  </div>
</template>

<script type = "text/javascript" >

import Todo from './Todo';

export default {
  props: ['todos'],
  components: {
    Todo,
  },
  methods: {
    deleteTodo(todo) {
      if (window.location.href.indexOf('pending_checked') > -1) {
        const isExecuted = confirm("If you delete in the 'pending only' mode, all completed tasks will be deleted too. Do you want to continue?");
        if (isExecuted === false) return;
      }
      const localstorageTodos = JSON.parse(localStorage.getItem('todos_all'));
      const index = localstorageTodos.findIndex(o => o.uid === todo.uid);
      if (index !== -1) {
        localstorageTodos.splice(index, 1);
      }
      localStorage.setItem('todos_all', JSON.stringify(localstorageTodos));
      this.$emit('check');
    },
    completeTodo(todo) {
      const todoIndex = this.todos.indexOf(todo);
      this.todos[todoIndex].done = true;
      this.$emit('save-todos');
      this.$emit('check');
    },
    uncompleteTodo(todo) {
      const todoIndex = this.todos.indexOf(todo);
      this.todos[todoIndex].done = false;
      this.$emit('save-todos');
      this.$emit('check');
    },
    saveTodos() {
      this.$emit('save-todos');
    },
  },
};

</script>
