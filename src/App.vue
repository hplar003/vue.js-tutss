<template>
  <div id="app">
    <Header />
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Todos from "./components/Todos";
import AddTodo from "./components/AddTodo";
import Header from "./components/layouts/Header";
import Axios from "axios";

export default {
  name: "app",
  components: {
    Todos,
    AddTodo,
    Header
  },
  data() {
    return {
      todos: []
    };
  },
  methods: {
    deleteTodo(id) {
      //this.todos = this.todos.filter(todo => todo.id !== id);
      Axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        .then(res => {
          this.todos = this.todos.filter(todo => todo.id !== id);
          return res;
        })
        .catch(error => {
          return Promise.reject(error);
        });
    },
    addTodo(newTodo) {
      // deconstruct the newTodo object
      const { title, completed } = newTodo;
      //this.todos = [...this.todos, newTodo];
      Axios.post("https://jsonplaceholder.typicode.com/todos", {
        title,
        completed
      })
        .then(res => (this.todos = [...this.todos, res.data]))
        .catch(error => {
          return Promise.reject(error);
        });
    }
  },
  created() {
    Axios.get("https://jsonplaceholder.typicode.com/todos?_limit=10")
      .then(res => (this.todos = res.data))
      .catch(error => {
        return Promise.reject(error);
      });
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
}
.btn {
  display: inline;
  border: none;
  background: rgb(29, 180, 218);
  color: #333;
  padding: 7px 20px;
  font-size: 1rem;
  text-transform: uppercase;
  font-weight: 700;
  letter-spacing: 1px;
  cursor: pointer;
}
.btn:hover {
  background: rgb(24, 146, 177);
  color: rgb(240, 236, 236);
}
</style>
