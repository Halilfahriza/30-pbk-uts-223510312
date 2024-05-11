<template>
  <div class="wrapper">
    <nav class="navbar navbar-expand-lg bg-body-tertiary">
      <div class="container-fluid">
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav">
            <li class="nav-item">
              <a class="nav-link active" aria-current="page" href="#">To-Do List</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="#">Post</a>
            </li>
          </ul>
        </div>
      </div>
    </nav>

    <div class="container py-5">
      <div class="row d-flex justify-content-center align-items-center">
        <div class="col-10">
          <div class="card rounded">
            <div class="card-body p-5">
              <h3 class="mb-3 fw-bold">
                To-Do List
                <span class="badge bg-primary rounded-pill ms-3 fs-6 fw-normal">{{ totalTodo }} task</span>
              </h3>

              <form @submit.prevent="addTodo" class="row align-items-center mb-3 gy-2">
                <div class="col-sm-12 col-md-10">
                  <div class="form-floating">
                    <input type="text" class="form-control fs-4" id="floatingInput" placeholder="Todos" autocomplete="off" v-model="todo" />
                    <label for="floatingInput">APA RENCANA KEGIATANMU HARI INI?</label>
                  </div>
                </div>
                <div class="col-sm-12 col-md-2">
                  <div class="d-grid">
                    <button type="submit" class="btn btn-primary btn-lg" :disabled="todo.length === 0">
                      <font-awesome-icon icon="circle-plus" />
                      Daftar Rencana Hari Ini
                    </button>
                  </div>
                </div>
              </form>

              <List :todos="todos" @deleteTodo="deleteTodo" @doneTodo="doneTodo" @updateTodo="updateTodo" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import List from "./components/List.vue";

export default {
  components: { List },
  data() {
    return {
      todo: "",
      todos: [],
    };
  },
  mounted() {
    if (localStorage.getItem("todos") !== null) {
      this.todos = JSON.parse(localStorage.getItem("todos"));
    }
  },
  methods: {
    addTodo() {
      if (this.todo.trim() !== "") {
        this.todos.unshift({
          activity: this.todo,
          isDone: false,
          editing: false, // Added to track editing state
        });
        this.todo = "";
        this.saveToLocalStorage();
      }
    },
    deleteTodo(indexDelete) {
      this.todos.splice(indexDelete, 1);
      this.saveToLocalStorage();
    },
    doneTodo(indexDone) {
      this.todos[indexDone].isDone = !this.todos[indexDone].isDone;
      this.saveToLocalStorage();
    },
    updateTodo(index, newTodo) {
      this.todos[index].activity = newTodo;
      this.saveToLocalStorage();
    },
    saveToLocalStorage() {
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },
  },
  computed: {
    totalTodo() {
      return this.todos.length;
    },
  },
};
</script>

<style>
@import "./assets/styles.css";
</style>
