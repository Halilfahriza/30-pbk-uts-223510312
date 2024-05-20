<template>
    <li class="list-group-item">
      <div class="row gy-2">
        <div class="col-sm-12 col-md-auto me-auto align-self-center fs-5" :class="{ 'text-decoration-line-through text-muted': todo.isDone }" v-if="!editing">
          <slot name="todo-content">{{ todo.activity }}</slot>
        </div>
        <div class="col-sm-12 col-md-auto" v-else>
          <input v-model="editedText" class="form-control" @keyup.enter="saveEdit" @keyup.escape="cancelEdit">
        </div>
        <div class="col-sm-12 col-md-auto">
          <div class="row gx-2">
            <div class="col-auto">
              <button class="btn" :class="[todo.isDone ? 'btn-outline-secondary' : 'btn-outline-success']" @click="toggleDone">
                <font-awesome-icon icon="circle-check" />
              </button>
            </div>
            <div class="col-auto">
              <button class="btn btn-outline-danger" @click="deleteTodo">
                <font-awesome-icon icon="eraser" />
              </button>
            </div>
            <div class="col-auto">
              <slot name="edit-buttons">
                <button class="btn btn-outline-primary" v-if="!editing" @click="editTodo">
                  <font-awesome-icon icon="edit" />
                </button>
                <button class="btn btn-outline-secondary" v-else @click="cancelEdit">
                  <font-awesome-icon icon="times" />
                </button>
                <button class="btn btn-outline-secondary" v-if="editing" @click="saveEdit">
                  <font-awesome-icon icon="check" />
                </button>
              </slot>
            </div>
          </div>
        </div>
      </div>
    </li>
  </template>
  
  <script>
  export default {
    props: {
      todo: {
        type: Object,
        required: true
      }
    },
    data() {
      return {
        editing: false,
        editedText: this.todo.activity
      };
    },
    methods: {
      editTodo() {
        this.editing = true;
      },
      cancelEdit() {
        this.editing = false;
        this.editedText = this.todo.activity;
      },
      saveEdit() {
        const trimmedText = this.editedText.trim();
        if (trimmedText !== "") {
          this.$emit("update", trimmedText);
          this.editing = false;
        }
      },
      deleteTodo() {
        this.$emit("delete");
      },
      toggleDone() {
        this.$emit("toggle-done");
      }
    }
  };
  </script>
  
  <style scoped>
  </style>