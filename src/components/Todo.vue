<template>
  <div class='ui centered card'>
    <div class="content" v-if="!isEditing" @click="toggleOpenForm">
      <div v-bind:class="{ done: todo.done, pending: !todo.done}" class="todo_title">
          {{ todo.title }}
            <span class='right floated trash icon' @click="deleteTodo(todo)">
          <i class='trash icon'></i>
        </span>
      </div>
    </div>
    <div class="content" v-if="isOpen">
      <div class='ui form'>
        <div class='field'>
          <label>Project info</label>
          <input type='text' v-model="todo.project" readonly />
        </div>
        <div class='field'>
          <label>Responsible person</label>
          <input type='text' v-model="todo.responsible" readonly />
        </div>
        <div class='field'>
          <label>Status (press to change)</label>
          <div class='ui bottom attached green basic button' v-if="!isEditing &&todo.done" @click="uncompleteTodo(todo)">
              Completed
          </div>
          <div class='ui bottom attached red basic button' v-if="!isEditing && !todo.done" @click="completeTodo(todo)">
              Pending
          </div>
        </div>
        <div class='field'>
        <label>Actions</label>
          <div class='extra content'>
            <button class='ui basic blue button' @click="showEditForm">
            Edit
            <span class='right floated edit icon'>
            <i class='edit icon'></i>
          </span>
          </button>
            <button class='ui basic right floated blue button' @click="deleteTodo(todo)">
            Delete
            <span class='right floated trash icon'>
            <i class='trash icon'></i>
          </span>
          </button>
          </div>
        </div>
        <div class='ui two button attached buttons'>
          <button class='ui basic blue button' @click="hideOpenForm">
            Close X
          </button>
        </div>
      </div>
    </div>
    <div class="content" v-if="isEditing">
      <div class='ui form'>
        <div>Editing {{todo.title}}</div>
        <div class='field'>
          <label>Title</label>
          <input type='text' v-model="todo.title" />
        </div>
        <div class='field'>
          <label>Project</label>
          <input type='text' v-model="todo.project" />
        </div>
        <div class='field'>
          <label>Responsible person</label>
          <input type='text' v-model="todo.responsible" />
        </div>
        <div class='ui two button attached buttons'>
          <button class='ui basic blue button' @click="hideEditForm">
            Close X
          </button>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  props: ['todo'],
  data() {
    return {
      isOpen: false,
      isEditing: false,
      localstorageTodos: [],
    };
  },
  methods: {
    toggleOpenForm() {
      if (this.isOpen) {
        this.isOpen = false;
      } else {
        this.isOpen = true;
      }
    },
    showEditForm() {
      this.isOpen = false;
      this.isEditing = true;
    },
    hideOpenForm() {
      this.isOpen = false;
    },
    hideEditForm() {
      this.isEditing = false;
      this.$emit('save-todos');
    },
    deleteTodo(todo) {
      this.$emit('delete-todo', todo);
    },
    completeTodo(todo) {
      this.$emit('complete-todo', todo);
    },
    uncompleteTodo(todo) {
      this.$emit('uncomplete-todo', todo);
    },
  },
};
</script>
