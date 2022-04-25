<template>
  <div class='ui basic content center aligned segment'>
    <label for="sort">Sort by:</label>
    <select name="sort" id="sort" @change="sortTodos($event)">
        <option value="">Choose the option</option>
        <option value="title_alphab_norm">By title: from A to Z</option>
        <option value="title_alphab_reverse">By title: from Z to A</option>
        <option value="old_first">Old first</option>
        <option value="new_first">New first</option>
    </select>
      <button v-bind:class="{ filtered: filterChecked === true}" class="btn pending_filter_btn">
      <input type="checkbox" id="pending_only" name="pending_only" @change="filterTodos($event)">
      <label for="pending_only">Pending only</label>
      </button>
  </div>
</template>

<script>

import Todo from './Todo';

export default {
  props: {
    localstorageTodos: {
      type: Array,
      required: true,
    },
  },
  components: {
    Todo,
  },
  methods: {
    checkSort() {
      if (window.location.href.indexOf('pending_checked') > -1) {
        document.getElementById('pending_only').checked = true;
      }
      if (JSON.parse(localStorage.getItem('pending_checked')) === true) {
        document.getElementById('pending_only').checked = true;
      } else {
        document.getElementById('pending_only').checked = false;
      }
      if (localStorage.getItem('todos_all')) {
        this.localstorageTodosSort = JSON.parse(localStorage.getItem('todos_all'));
      }
      if (window.location.href.indexOf('pending_checked') < 0) {
        if (localStorage.getItem('todos_with_completed') > 0) {
          this.localstorageTodosSort = JSON.parse(localStorage.getItem('todos_with_completed'));
        }
      }
      if (localStorage.getItem('todos_with_completed')) {
        this.localstorageTodosSortWithCompleted = JSON.parse(localStorage.getItem('todos_all'));
      }
      if (document.getElementById('pending_only').checked) {
        this.filterChecked = true;
      } else {
        this.filterChecked = false;
      }
    },
    sortTodos(event) {
      if (event.target.value === 'title_alphab_norm') {
        this.localstorageTodosSort.sort((a, b) => ((a.title > b.title) ? 1 : ((b.title > a.title) ? -1 : 0)));
        localStorage.setItem('todos_all', JSON.stringify(this.localstorageTodosSort));
      }
      if (event.target.value === 'title_alphab_reverse') {
        this.localstorageTodosSort.sort((a, b) => ((a.title < b.title) ? 1 : ((b.title < a.title) ? -1 : 0)));
        localStorage.setItem('todos_all', JSON.stringify(this.localstorageTodosSort));
      }
      if (event.target.value === 'old_first') {
        this.localstorageTodosSort.sort((a, b) => ((a.uid > b.uid) ? 1 : ((b.uid > a.uid) ? -1 : 0)));
        localStorage.setItem('todos_all', JSON.stringify(this.localstorageTodosSort));
      }
      if (event.target.value === 'new_first') {
        this.localstorageTodosSort.sort((a, b) => ((a.uid < b.uid) ? 1 : ((b.uid < a.uid) ? -1 : 0)));
        localStorage.setItem('todos_all', JSON.stringify(this.localstorageTodosSort));
      }
      this.$emit('check');
    },
    filterTodos(event) {
      if (event.target.id === 'pending_only') {
        if (document.getElementById('pending_only').checked) {
          localStorage.setItem('pending_checked', 'true');
          this.filterChecked = true;
          localStorage.setItem('todos_with_completed', JSON.stringify(this.localstorageTodosSort));
          const onlyPending = this.localstorageTodosSort.filter(todo => todo.done === false);
          localStorage.setItem('todos_all', JSON.stringify(onlyPending));
          window.location.href = `${window.location.href}#pending_checked=true`;
        } else if (localStorage.getItem('todos_with_completed')) {
          this.filterChecked = false;
          localStorage.setItem('todos_all', localStorage.getItem('todos_with_completed'));
          window.location.href = window.location.href.substring(0, window.location.href.indexOf('#') + 1);
          this.$emit('check');
          localStorage.setItem('pending_checked', 'false');
        } else {
          this.filterChecked = false;
          localStorage.setItem('pending_checked', 'false');
        }
        this.$emit('check');
      }
    },
  },
  data() {
    return {
      localstorageTodosWithCompleted: [],
      localstorageTodosSort: this.localstorageTodos,
      filterChecked: Boolean,
    };
  },
};
</script>
