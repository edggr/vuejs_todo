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
      if (document.getElementById('pending_only').checked) {
        this.filterChecked = true;
      } else {
        this.filterChecked = false;
      }
      if (localStorage.getItem('todos_all')) {
        this.localstorageTodosSort = JSON.parse(localStorage.getItem('todos_all'));
      }
    },
    sortTodos(event) {
      this.$emit('refresh');
      this.$emit('check');
      if (event.target.value === 'title_alphab_norm') {
        this.$emit('sort-list', this.localstorageTodosSort.sort((a, b) => a.title.localeCompare(b.title)));
      }
      if (event.target.value === 'title_alphab_reverse') {
        this.$emit('sort-list', this.localstorageTodosSort.sort((a, b) => b.title.localeCompare(a.title)));
      }
      if (event.target.value === 'old_first') {
        this.$emit('sort-list', this.localstorageTodosSort.sort((a, b) => ((a.uid > b.uid) ? 1 : ((b.uid > a.uid) ? -1 : 0))));
      }
      if (event.target.value === 'new_first') {
        this.$emit('sort-list', this.localstorageTodosSort.sort((a, b) => ((a.uid < b.uid) ? 1 : ((b.uid < a.uid) ? -1 : 0))));
      }
    },
    filterTodos() {
      if (document.getElementById('pending_only').checked) {
        this.filterChecked = true;
        this.$emit('filter-pending', this.localstorageTodosSort.filter(todo => todo.done === false));
        document.getElementById('completed_tasks').style.visibility = 'hidden';
      } else {
        this.filterChecked = false;
        this.localstorageTodosSort = JSON.parse(localStorage.getItem('todos_all'));
        this.$emit('refresh');
        this.$emit('check');
        document.getElementById('completed_tasks').style.visibility = 'visible';
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
