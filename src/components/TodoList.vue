<template>
    <ul class="list-group list-group-flush">
      <TodoItem
        v-for="(todo, index) in todos"
        :key="index"
        :todo="todo"
        @delete="deleteTodo(index)"
        @toggle-done="toggleDone(index)"
        @update="updateTodo(index, $event)"
      >
        <template v-slot:todo-content>
          {{ todo.activity }}
        </template>
        <template v-slot:edit-buttons>
          <button class="btn btn-outline-primary" v-if="!editing" @click="editTodo">
            <font-awesome-icon icon="edit" />
          </button>
          <button class="btn btn-outline-secondary" v-else @click="cancelEdit">
            <font-awesome-icon icon="times" />
          </button>
          <button class="btn btn-outline-secondary" v-if="editing" @click="saveEdit">
            <font-awesome-icon icon="check" />
          </button>
        </template>
      </TodoItem>
    </ul>
  </template>
  
  <script>
  import TodoItem from "./TodoItem.vue";
  
  export default {
    components: {
      TodoItem
    },
    props: {
      todos: {
        type: Array,
        required: true
      }
    },
    methods: {
      deleteTodo(index) {
        this.$emit("delete-todo", index);
      },
      toggleDone(index) {
        this.$emit("toggle-done", index);
      },
      updateTodo(index, text) {
        this.$emit("update-todo", index, text);
      }
    }
  };
  </script>
  
  <style scoped>
  </style>