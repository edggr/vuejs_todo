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
      <input type="checkbox" id="pending_only" name="pending_only" @change="filterTodos()">
      <label for="pending_only">Pending only</label>
      </button>
  </div>
</template>

<script>

export default {
  props: {
    localstorageTodos: {
      type: Array,
      required: true,
    },
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
      if (!document.getElementById('pending_only').checked) {
        if (localStorage.getItem('todos_all')) {
          this.localstorageTodosSort = JSON.parse(localStorage.getItem('todos_all'));
        }
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
    filterTodos() {
      if (document.getElementById('pending_only').checked) {
        localStorage.setItem('pending_checked', 'true');
        this.filterChecked = true;
        this.$emit('filter-pending', this.localstorageTodosSort.filter(todo => todo.done === false));
      } else {
        this.filterChecked = false;
        localStorage.setItem('pending_checked', 'false');
        this.localstorageTodosSort = JSON.parse(localStorage.getItem('todos_all'));
      }
      this.$emit('check');
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
