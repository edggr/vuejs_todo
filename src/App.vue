<template>
  <div>
    <count-todos v-bind:localstorageTodos="localstorageTodos"></count-todos>
    <download-todos :localstorageTodos="localstorageTodos"></download-todos>
    <todo-list-container v-bind:todos="todos" :localstorageTodos="localstorageTodos" @check="check" @refresh="refresh" @save-todos="saveTodos" @sort-list-container="sortList" @filter-list-container="filterList" ref="listContainer"></todo-list-container>
    <paginate-todos v-bind:localstorageTodos="localstorageTodos" @check="check" @refresh="refresh" ref="checkPagination"></paginate-todos>
  </div>
</template>

<script>
import CountTodos from './components/CountTodos';
import DownloadTodos from './components/DownloadTodos';
import TodoListContainer from './components/TodoListContainer';
import PaginateTodos from './components/PaginateTodos';

export default {
  name: 'app',
  components: {
    CountTodos,
    DownloadTodos,
    TodoListContainer,
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
        this.$refs.listContainer.addTodo('Todo B', 'Project B', 'Donatello');
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
      this.$refs.listContainer.checkSort();
      this.$refs.checkPagination.checkPagination();
    },
    saveTodos() {
      localStorage.setItem('todos_all', JSON.stringify(this.localstorageTodos));
    },
    sortList(sortedList) {
      this.localstorageTodos = sortedList;
    },
    filterList(filteredList) {
      this.localstorageTodos = filteredList;
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
