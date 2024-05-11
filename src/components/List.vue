<script>
export default {
  props: {
    todos: {
      type: Array,
      default: [],
    },
  },
  data() {
    return {
      editedTodoIndex: null,
      editedTodoText: "",
    };
  },
  methods: {
    deleteTodo(index) {
      this.$emit("deleteTodo", index);
    },
    doneTodo(index) {
      this.$emit("doneTodo", index);
    },
    editTodo(index, text) {
      this.editedTodoIndex = index;
      this.editedTodoText = text;
    },
    cancelEdit() {
      this.editedTodoIndex = null;
      this.editedTodoText = "";
    },
    saveEdit(index) {
      if (this.editedTodoText.trim() !== "") {
        this.$emit("updateTodo", index, this.editedTodoText);
        this.editedTodoIndex = null;
        this.editedTodoText = "";
      }
    },
  },
};
</script>

<template>
  <ul class="list-group list-group-flush">
    <li v-for="(todo, index) in todos" :key="index" class="list-group-item">
      <div class="row gy-2">
        <div class="col-sm-12 col-md-auto me-auto align-self-center fs-5"
             :class="{ 'text-decoration-line-through text-muted': todo.isDone }"
             v-if="editedTodoIndex !== index">
          {{ todo.activity }}
        </div>
        <div class="col-sm-12 col-md-auto"
             v-else>
          <input v-model="editedTodoText" class="form-control" @keyup.enter="saveEdit(index)" @keyup.escape="cancelEdit">
        </div>
        <div class="col-sm-12 col-md-auto">
          <div class="row gx-2">
            <div class="col-auto">
              <button class="btn"
                      :class="[
                        todo.isDone ? 'btn-outline-secondary' : 'btn-outline-success',
                      ]"
                      @click="doneTodo(index)">
                <font-awesome-icon icon="circle-check" />
              </button>
            </div>
            <div class="col-auto">
              <button class="btn"
                      :class="[
                        todo.isDone ? 'btn-outline-secondary' : 'btn-outline-danger',
                      ]"
                      @click="deleteTodo(index)">
                <font-awesome-icon icon="eraser" />
              </button>
            </div>
            <div class="col-auto">
              <button class="btn btn-outline-primary"
                      v-if="editedTodoIndex !== index"
                      @click="editTodo(index, todo.activity)">
                <font-awesome-icon icon="edit" />
              </button>
              <button class="btn btn-outline-secondary"
                      v-else
                      @click="cancelEdit">
                <font-awesome-icon icon="times" />
              </button>
              <button class="btn btn-outline-secondary"
                      v-if="editedTodoIndex === index"
                      @click="saveEdit(index)">
                <font-awesome-icon icon="check" />
              </button>
            </div>
          </div>
        </div>
      </div>
    </li>
  </ul>
</template>
