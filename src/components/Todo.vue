<template>
  <div>
    <h1>Selamat Datang</h1>
    <div>Berikut daftar kerja kita :</div>
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        {{ todo.todo }} <button @click="deleteTodo(todo.id)">-</button>
      </li>
    </ul>
    <input v-model="todo" type="text" />
    <button @click="addTodos">Add Data</button>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "app",
  data() {
    return {
      todos: [],
      todo:""
    };
  },
  mounted: function () {
    this.getTodos();
  },
  methods: {
    getTodos() {
      const username = localStorage.getItem("usr");
      const password = localStorage.getItem("pwd");

      axios.get("http://localhost:4000/todo", {headers: { username, password }}).then((result) => {
        this.todos = result.data;
        console.log(result);
      });
    },
    addTodos() {
      let addItem = { todo: this.todo };
      const username = localStorage.getItem("usr");
      const password = localStorage.getItem("pwd");

      console.log(addItem);
      axios.post("http://localhost:4000/todo", addItem, {headers: { username, password }}).then(() => {
        this.getTodos();
      });
    },
    deleteTodo(id) {
      const username = localStorage.getItem("usr");
      const password = localStorage.getItem("pwd");

      axios
      .delete(`http://localhost:4000/todo/${id}`, {
        headers: { username, password },
        })
        .then(() => {
        this.getTodos();
      });
    },
  },
};
</script>