<template>
  <div class='ui basic content center aligned segment'>
    <button class='ui basic button icon' @click="openForm" v-if="!isCreating">
      <i class='plus icon'></i>
    </button>
    <div class='ui centered card' v-if="isCreating">
      <div class='content'>
        <div class='ui form'>
          <div class='field'>
            <label>Title</label>
            <input v-model="titleText" type='text' ref='title' defaultValue="" v-on:keyup="validateForm" />
          </div>
          <div class='field'>
            <label>Project</label>
            <input v-model="projectText" type='text' ref='project' defaultValue="" v-on:keyup="validateForm" />
          </div>
          <div class='field'>
            <label>Responsible person</label>
            <input v-model="responsibleText" type='text' ref='project' defaultValue="" v-on:keyup="validateForm" />
          </div>
          <div class='ui two button attached buttons'>
            <button class='ui basic blue button' @click="sendForm" :disabled="isDisabled" v-if="!buttonDisabled">
              Create
            </button>
            <button class='ui basic red button' @click="closeForm">
              Cancel
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      titleText: '',
      projectText: '',
      responsibleText: '',
      isCreating: false,
      buttonDisabled: true,
    };
  },
  computed: {
    isDisabled() {
      return this.buttonDisabled === true;
    },
  },
  methods: {
    openForm() {
      this.isCreating = true;
    },
    closeForm() {
      this.isCreating = false;
    },
    validateForm() {
      if (this.isCreating === true && this.titleText.length > 0 && this.projectText.length > 0 && this.responsibleText.length > 0) {
        this.buttonDisabled = false;
      } else {
        this.buttonDisabled = true;
      }
    },
    sendForm() {
      if (this.titleText.length > 0 && this.projectText.length > 0 && this.responsibleText.length > 0) {
        const title = this.titleText;
        const project = this.projectText;
        const responsible = this.responsibleText;
        this.$emit('add-todo', title, project, responsible);
        this.titleText = '';
        this.projectText = '';
        this.responsibleText = '';
        this.buttonDisabled = true;
      }
      this.isCreating = false;
      this.$emit('check');
    },
  },
};
</script>
