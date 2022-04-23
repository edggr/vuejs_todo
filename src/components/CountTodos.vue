<template>
  <div>
    <p>Completed Tasks: {{localstorageTodosCount.filter(todo => {return todo.done === true}).length}}</p>
    <p>Pending Tasks: {{localstorageTodosCount.filter(todo => {return todo.done === false}).length}}</p>
  </div>
</template>

<script type = "text/javascript" >

import Paginate from 'vuejs-paginate';
import Todo from './Todo';

export default {
  props: ['localstorageTodos'],
  components: {
    Paginate,
    Todo,
  },
  mounted() {
    if (localStorage.getItem('todos_all')) {
      this.localstorageTodosCount = JSON.parse(localStorage.getItem('todos_all'));
    }
  },
  data() {
    return {
      localstorageTodosCount: this.localstorageTodos,
    };
  },
};

</script>
