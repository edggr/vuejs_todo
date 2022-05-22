<template>
  <div>
    <sort-todos :localstorageTodos="localstorageTodos" @check="check" @refresh="refresh"  @filter-pending="filterPending" @sort-list="sort" ref="checkSort"></sort-todos>
    <todo-list v-bind:todos="todos" @check="check" @save-todos="saveTodos" @refresh="refresh"></todo-list>
    <create-todo @add-todo="addTodo" @save-todos="saveTodos" @refresh="refresh"></create-todo>
  </div>
</template>

<script>
import SortTodos from './TodoListContainer/SortTodos';
import TodoList from './TodoListContainer/TodoList';
import CreateTodo from './TodoListContainer/CreateTodo';

export default {
  name: 'app',
  components: {
    SortTodos,
    TodoList,
    CreateTodo,
  },
  props: {
    localstorageTodos: {
      type: Array,
      required: true,
    },
    todos: {
      type: Array,
      required: true,
    },
  },
  methods: {
    refresh() {
      this.$emit('refresh');
    },
    check() {
      this.$emit('check');
    },
    checkSort() {
      this.$refs.checkSort.checkSort();
    },
    saveTodos() {
      this.$emit('save-todos');
    },
    addTodo(newtitle, newproject, newresponsible) {
      const pagesBeforeAdding = Math.ceil(this.localstorageTodos.length / 5);
      let newUid = 1;
      if (this.localstorageTodos.length > 0) {
        const lastTodo = this.localstorageTodos.length - 1;
        newUid = this.localstorageTodos[lastTodo].uid + 1;
      }
      this.localstorageTodos.push({
        uid: newUid,
        title: newtitle,
        project: newproject,
        responsible: newresponsible,
        done: false,
      });
      if (pagesBeforeAdding < Math.ceil(this.localstorageTodos.length / 5)) {
        this.currentPage = Math.ceil(this.localstorageTodos.length / 5);
        localStorage.setItem('pagination', this.currentPage);
      }
      this.$emit('save-todos');
      this.$emit('check');
    },
    sort(sortedTodos) {
      if (document.getElementById('sort')) {
        this.localstorageTodosContainerSortFilter = sortedTodos;
        this.$emit('sort-list-container', this.localstorageTodosContainerSortFilter);
      } else {
        this.localstorageTodosContainerSortFilter = JSON.parse(localStorage.getItem('todos_all'));
        this.$emit('sort-list-container', this.localstorageTodosContainerSortFilter);
      }
      this.$emit('check');
    },
    filterPending(filteredtodos) {
      if (document.getElementById('pending_only').checked) {
        this.localstorageTodosContainerSortFilter = filteredtodos;
        this.$emit('filter-list-container', this.localstorageTodosContainerSortFilter);
      } else {
        this.localstorageTodosContainerSortFilter = JSON.parse(localStorage.getItem('todos_all'));
        this.$emit('filter-list-container', this.localstorageTodosContainerSortFilter);
      }
      this.$emit('check');
    },
  },
  data() {
    return {
      localstorageTodosContainerSortFilter: this.localstorageTodos,
    };
  },
};
</script>
