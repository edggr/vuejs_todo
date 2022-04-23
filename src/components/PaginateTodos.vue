<template>
  <div>
    <paginate
      v-model="pageNumber"
      :page-count="pagesInPagination"
      :click-handler="clickCallback"
      :prev-text="'Prev'"
      :next-text="'Next'"
      :container-class="'pagination'">
    </paginate>
  </div>
</template>

<script type = "text/javascript" >

import Paginate from 'vuejs-paginate';
import Todo from './Todo';


export default {
  props: ['localstorageTodos'],
  components: {
    paginate: Paginate,
    Todo,
  },
  mounted() {
    if (localStorage.getItem('todos_all')) {
      this.localstorageTodosPaginate = JSON.parse(localStorage.getItem('todos_all'));
    }

    if (window.location.href.indexOf('?') > -1) {
      const url = new URL(window.location.href);
      this.pageNumberFromURL = Number(url.searchParams.get('page'));
      this.pageNumber = this.pageNumberFromURL;
    }
    if (this.localstorageTodosPaginate) {
      this.pagesInPagination = Math.ceil(this.localstorageTodosPaginate.length / 5);
      if (this.pagesInPagination < 1) {
        this.pagesInPagination = 1;
      }
    }
    if (this.pageNumberFromURL > this.pagesInPagination && this.pageNumberFromURL > 1) {
      const searchParams = new URLSearchParams(window.location.search);
      searchParams.set('page', this.pagesInPagination);
      window.location.search = searchParams.toString();
    }
  },
  methods: {
    clickCallback(pageNumber) {
      if (this.pageNumberFromURL !== pageNumber) {
        const searchParams = new URLSearchParams(window.location.search);
        searchParams.set('page', pageNumber);
        window.location.search = searchParams.toString();
      }
    },
  },
  data() {
    return {
      pagesInPagination: 1,
      pageNumber: 1,
      pageNumberFromURL: '',
      localstorageTodosPaginate: this.localstorageTodos,
    };
  },
};

</script>

<style lang="css">
  .pagination{display:inline-block;padding-left:0;margin:20px 0;border-radius:4px}.pagination>li{display:inline}.pagination>li>a,.pagination>li>span{position:relative;float:left;padding:6px 12px;margin-left:-1px;line-height:1.42857143;color:#337ab7;text-decoration:none;background-color:#fff;border:1px solid #ddd}.pagination>li:first-child>a,.pagination>li:first-child>span{margin-left:0;border-top-left-radius:4px;border-bottom-left-radius:4px}.pagination>li:last-child>a,.pagination>li:last-child>span{border-top-right-radius:4px;border-bottom-right-radius:4px}.pagination>li>a:hover,.pagination>li>span:hover,.pagination>li>a:focus,.pagination>li>span:focus{z-index:3;color:#23527c;background-color:#eee;border-color:#ddd}.pagination>.active>a,.pagination>.active>span,.pagination>.active>a:hover,.pagination>.active>span:hover,.pagination>.active>a:focus,.pagination>.active>span:focus{z-index:2;color:#fff;cursor:default;background-color:#337ab7;border-color:#337ab7}.pagination>.disabled>span,.pagination>.disabled>span:hover,.pagination>.disabled>span:focus,.pagination>.disabled>a,.pagination>.disabled>a:hover,.pagination>.disabled>a:focus{color:#777;cursor:not-allowed;background-color:#fff;border-color:#ddd}.pagination-lg>li>a,.pagination-lg>li>span{padding:10px 16px;font-size:18px;line-height:1.3333333}.pagination-lg>li:first-child>a,.pagination-lg>li:first-child>span{border-top-left-radius:6px;border-bottom-left-radius:6px}.pagination-lg>li:last-child>a,.pagination-lg>li:last-child>span{border-top-right-radius:6px;border-bottom-right-radius:6px}.pagination-sm>li>a,.pagination-sm>li>span{padding:5px 10px;font-size:12px;line-height:1.5}.pagination-sm>li:first-child>a,.pagination-sm>li:first-child>span{border-top-left-radius:3px;border-bottom-left-radius:3px}.pagination-sm>li:last-child>a,.pagination-sm>li:last-child>span{border-top-right-radius:3px;border-bottom-right-radius:3px}.pager{padding-left:0;margin:20px 0;text-align:center;list-style:none}.pager li{display:inline}.pager li>a,.pager li>span{display:inline-block;padding:5px 14px;background-color:#fff;border:1px solid #ddd;border-radius:15px}
</style>
