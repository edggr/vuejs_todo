<template>
  <div>
    <div class="counter_container">
      <p id="completed_tasks">Completed Tasks: <span class="done">{{localstorageTodos.filter(todo => {return todo.done === true}).length}}</span></p>
      <p>Pending Tasks: <span class="pending">{{localstorageTodos.filter(todo => {return todo.done === false}).length}}</span></p>
    </div>
  </div>
</template>

<script type = "text/javascript" >

import Todo from './TodoListContainer/Todo';

export default {
  props: ['localstorageTodos'],
  components: {
    Todo,
  },
  mounted() {

  },
  data() {
    return {
    };
  },
};

</script>
