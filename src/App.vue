<template>
  <div>
    <count-todos v-bind:localstorageTodos="localstorageTodos"></count-todos>
    <download-todos :localstorageTodos="localstorageTodos"></download-todos>
    <sort-todos :localstorageTodos="localstorageTodos" @check="check" ref="checkSort"></sort-todos>
    <todo-list v-bind:todos="todos" @check="check" @save-todos="saveTodos"></todo-list>
    <create-todo @add-todo="addTodo" @save-todos="saveTodos"></create-todo>
    <paginate-todos :currentPage="currentPage" v-bind:localstorageTodos="localstorageTodos" @check="check" ref="checkPagination"></paginate-todos>
  </div>
</template>

<script>
import CountTodos from './components/CountTodos';
import DownloadTodos from './components/DownloadTodos';
import SortTodos from './components/SortTodos';
import TodoList from './components/TodoList';
import CreateTodo from './components/CreateTodo';
import PaginateTodos from './components/PaginateTodos';

export default {
  name: 'app',
  components: {
    CountTodos,
    DownloadTodos,
    SortTodos,
    TodoList,
    CreateTodo,
    PaginateTodos,
  },
  mounted() {
    this.check();
  },
  methods: {
    check() {
      if (localStorage.getItem('pagination')) {
        this.currentPage = localStorage.getItem('pagination');
      }
      if (localStorage.getItem('todos_all')) {
        this.localstorageTodos = JSON.parse(localStorage.getItem('todos_all'));
      }
      if (this.localstorageTodos) {
        const todosShowingRangeMax = this.currentPage * 5;
        const todosShowingRangeMin = (this.currentPage * 5) - 5;
        this.todos = this.localstorageTodos.slice(todosShowingRangeMin, todosShowingRangeMax);
      }
      if (this.todos.length < 1 && this.currentPage > 1) {
        this.currentPage = this.currentPage - 1;
        const todosShowingRangeMax = this.currentPage * 5;
        const todosShowingRangeMin = (this.currentPage * 5) - 5;
        this.todos = this.localstorageTodos.slice(todosShowingRangeMin, todosShowingRangeMax);
      }
      this.$refs.checkSort.checkSort();
      this.$refs.checkPagination.checkPagination();
    },
    paginateTodos() {
      localStorage.setItem('pagination_todos_quantity', this.todos.length);
    },
    saveTodos() {
      localStorage.setItem('todos_all', JSON.stringify(this.localstorageTodos));
      localStorage.setItem('todos_with_completed', JSON.stringify(this.localstorageTodos));
    },
    addTodo(newtitle, newproject, newresponsible) {
      let uidToExport = 1;
      if (this.localstorageTodos) {
        const lastTodo = this.localstorageTodos.length - 1;
        uidToExport = this.localstorageTodos[lastTodo].uid + 1;
      }
      JSON.stringify(localStorage.setItem('last_uid', uidToExport));
      if (localStorage.getItem('todos_all')) {
        this.localstorageTodos = JSON.parse(localStorage.getItem('todos_all'));
      }
      this.localstorageTodos.push({
        uid: uidToExport,
        title: newtitle,
        project: newproject,
        responsible: newresponsible,
        done: false,
      });
      this.saveTodos();
      this.check();
    },
  },
  data() {
    return {
      todos: [],
      localstorageTodos: [{
        uid: 1,
        title: 'Todo A',
        project: 'Project A',
        responsible: 'Leonardo',
        done: false,
      }, {
        uid: 2,
        title: 'Todo B',
        project: 'Project B',
        responsible: 'Raphael',
        done: false,
      }, {
        uid: 3,
        title: 'Todo C',
        project: 'Project C',
        responsible: 'Michelangelo',
        done: false,
      }, {
        uid: 4,
        title: 'Todo D',
        project: 'Project D',
        responsible: 'Donatello',
        done: false,
      }, {
        uid: 5,
        title: 'Todo A',
        project: 'Project A',
        responsible: 'Michelangelo',
        done: false,
      }, {
        uid: 6,
        title: 'Todo B',
        project: 'Project B',
        responsible: 'Leonardo',
        done: false,
      }, {
        uid: 7,
        title: 'Todo C',
        project: 'Project C',
        responsible: 'Leonardo',
        done: false,
      }, {
        uid: 8,
        title: 'Todo D',
        project: 'Project D',
        responsible: 'Donatello',
        done: false,
      }, {
        uid: 9,
        title: 'Todo A',
        project: 'Project A',
        responsible: 'Raphael',
        done: false,
      }, {
        uid: 10,
        title: 'Todo B',
        project: 'Project B',
        responsible: 'Raphael',
        done: false,
      }],
      currentPage: 1,
    };
  },
};
</script>

<style>

@import './assets/styles/styles.css';

</style>
