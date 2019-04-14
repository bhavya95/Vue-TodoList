<template>
  <div id="app">
    <AddTodo v-on:add-todo="addTodo"/>
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
  </div>
</template>

<script>
import Todos from "../components/Todos.vue";
import AddTodo from "../components/AddTodo.vue";
import axios from "axios";

export default {
  name: "Home",
  components: {
    Todos,
    AddTodo
  },
  data() {
    return {
      todos: []
    };
  },
  methods: {
    async deleteTodo(id) {
      try {
        const response = await axios.delete(
          `https://jsonplaceholder.typicode.com/todos/${id}`
        );
        if (response.status === 200) {
          this.todos = this.todos.filter(todo => todo.id !== id);
        }
      } catch (e) {
        console.log(e);
      }
    },
    async addTodo(newTodo) {
      const { title, completed } = newTodo;
      try {
        const response = await axios.post(
          "https://jsonplaceholder.typicode.com/todos",
          {
            title,
            completed
          }
        );
        this.todos.push(response.data);
      } catch (e) {
        console.log(e);
      }
    }
  },
  async created() {
    try {
      const res = await axios.get(
        "https://jsonplaceholder.typicode.com/todos?_limit=5"
      );
      this.todos = res.data;
    } catch (e) {
      console.log(e);
    }
  }
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Arial, Helvetica, sans-serif;
  line-height: 1.4;
}

.btn {
  display: inline-block;
  border: none;
  background: #555;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}
.btn:hover {
  background: #666;
}
</style>
