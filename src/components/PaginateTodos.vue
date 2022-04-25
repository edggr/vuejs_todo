<template>
  <div>
    <div class="pages_list_container">
      <ul class="pages_list">
        <li><a @click="clickCallback(prevPage)" href="#">Prev</a></li>
        <li v-for="page in pages" @click="clickCallback(page)" :key="page.id"><a v-bind:class="{ active: currentPageFromParent === page }" href="#">{{page}}</a></li>
        <li><a @click="clickCallback(nextPage)" href="#">Next</a></li>
      </ul>
    </div>
  </div>
</template>

<script type = "text/javascript" >

import Todo from './Todo';


export default {
  props: {
    localstorageTodos: {
      type: Array,
      required: true,
    },
    currentPage: {
      required: true,
    },
  },
  components: {
    Todo,
  },
  methods: {
    checkPagination() {
      if (localStorage.getItem('todos_all')) {
        this.localstorageTodosPaginate = JSON.parse(localStorage.getItem('todos_all'));
      }
      if (this.localstorageTodosPaginate) {
        this.pagesInPagination = Math.ceil(this.localstorageTodosPaginate.length / 5);
        if (this.pagesInPagination < 1) {
          this.pagesInPagination = 1;
        }
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
      if (this.currentPageFromParent > 1) {
        this.prevPage = this.currentPageFromParent - 1;
      } else {
        this.prevPage = 1;
      }
      if (this.currentPageFromParent < this.pagesInPagination) {
        this.nextPage = this.currentPageFromParent + 1;
      } else {
        this.nextPage = this.pagesInPagination;
      }
    },
    clickCallback(currentPage) {
      if (this.currentPageFromParent !== currentPage) {
        localStorage.setItem('pagination', currentPage);
        this.currentPageFromParent = currentPage;
        this.$emit('check');
      }
    },
  },
  data() {
    return {
      pagesInPagination: 1,
      currentPageFromParent: this.currentPage,
      prevPage: '',
      nextPage: '',
      pages: [],
      localstorageTodosPaginate: this.localstorageTodos,
    };
  },
};

</script>

