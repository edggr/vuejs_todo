<template>
  <div>
    <p>Completed Tasks: <span class="done">{{localstorageTodosCount.filter(todo => {return todo.done === true}).length}}</span></p>
    <p>Pending Tasks: <span class="pending">{{localstorageTodosCount.filter(todo => {return todo.done === false}).length}}</span></p>
  </div>
</template>

<script type = "text/javascript" >

import Todo from './Todo';

export default {
  props: ['localstorageTodos'],
  components: {
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
