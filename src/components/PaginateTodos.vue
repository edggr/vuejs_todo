<template>
  <div>
    <div class="pages_list_container" v-if="localstorageTodosPaginate.length > 0">
      <ul class="pages_list">
        <li><a @click="clickCallback(prevPage)" href="#">Prev</a></li>
        <li v-for="page in pages" @click="clickCallback(page)" :key="page.id"><a v-bind:class="{ active: currentPage === page }" href="#">{{page}}</a></li>
        <li><a @click="clickCallback(nextPage)" href="#">Next</a></li>
      </ul>
    </div>
  </div>
</template>

<script type = "text/javascript" >

import Todo from './TodoListContainer/Todo';


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
    checkPagination() {
      if (!document.getElementById('pending_only').checked) {
        if (localStorage.getItem('todos_all')) {
          this.localstorageTodosPaginate = JSON.parse(localStorage.getItem('todos_all'));
        }
      }
      if (this.localstorageTodosPaginate) {
        this.pagesInPagination = Math.ceil(this.localstorageTodosPaginate.length / 5);
        if (this.pagesInPagination < 1) {
          this.pagesInPagination = 1;
        }
      }
      if (localStorage.getItem('pagination')) {
        this.currentPage = JSON.parse(localStorage.getItem('pagination'));
      }
      if (this.currentPage > Math.ceil(this.localstorageTodosPaginate.length / 5)) {
        this.currentPage = Math.ceil(this.localstorageTodosPaginate.length / 5);
        localStorage.setItem('pagination', this.currentPage);
        this.clickCallback(this.currentPage);
      }
      this.createPages();
      this.linkPrevNextPages();
    },
    createPages() {
      this.pages = [];
      for (let counter = 1; counter < this.pagesInPagination + 1; counter += 1) {
        this.pages.push(counter);
      }
    },
    linkPrevNextPages() {
      if (this.currentPage > 1) {
        this.prevPage = this.currentPage - 1;
      } else {
        this.prevPage = 1;
      }
      if (this.currentPage < this.pagesInPagination) {
        this.nextPage = this.currentPage + 1;
      } else {
        this.nextPage = this.pagesInPagination;
      }
    },
    clickCallback(currentPage) {
      if (this.currentPage !== currentPage) {
        localStorage.setItem('pagination', currentPage);
        this.currentPage = currentPage;
        this.$emit('check');
      }
    },
  },
  data() {
    return {
      pagesInPagination: 1,
      currentPage: 1,
      prevPage: '',
      nextPage: '',
      pages: [],
      localstorageTodosPaginate: this.localstorageTodos,
    };
  },
};

</script>

