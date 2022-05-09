<template>
  <div>
    <count-todos v-bind:localstorageTodos="localstorageTodos"></count-todos>
    <download-todos :localstorageTodos="localstorageTodos"></download-todos>
    <sort-todos :localstorageTodos="localstorageTodos" @check="check" @refresh="refresh" @filter-pending="filterPending" @sort-list="sort" ref="checkSort"></sort-todos>
    <todo-list v-bind:todos="todos" @check="check" @save-todos="saveTodos" @refresh="refresh"></todo-list>
    <create-todo @add-todo="addTodo" @save-todos="saveTodos" @refresh="refresh"></create-todo>
    <paginate-todos v-bind:localstorageTodos="localstorageTodos" @check="check" @refresh="refresh" ref="checkPagination"></paginate-todos>
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
    this.refresh();
    this.localstorageTodos.sort((a, b) => ((a.uid > b.uid) ? 1 : ((b.uid > a.uid) ? -1 : 0)));
    this.check();
  },
  methods: {
    refresh() {
      if (localStorage.getItem('todos_all')) {
        this.localstorageTodos = JSON.parse(localStorage.getItem('todos_all'));
      } else {
        this.addTodo('Todo B', 'Project B', 'Donatello');
      }
    },
    check() {
      if (localStorage.getItem('pagination')) {
        this.currentPage = localStorage.getItem('pagination');
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
    saveTodos() {
      localStorage.setItem('todos_all', JSON.stringify(this.localstorageTodos));
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
      this.saveTodos();
      this.check();
    },
    sort(sortedTodos) {
      if (document.getElementById('sort')) {
        this.localstorageTodos = sortedTodos;
      } else {
        this.localstorageTodos = JSON.parse(localStorage.getItem('todos_all'));
      }
      this.check();
    },
    filterPending(filteredtodos) {
      if (document.getElementById('pending_only').checked) {
        this.localstorageTodos = filteredtodos;
      } else {
        this.localstorageTodos = JSON.parse(localStorage.getItem('todos_all'));
      }
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
      }],
      currentPage: 1,
      sorted: 0,
    };
  },
};
</script>

<style>

@import './assets/styles/styles.css';

</style>
