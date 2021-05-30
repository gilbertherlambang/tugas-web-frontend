<template>
  <div>
    <h1>Selamat Datang</h1>
    <div>Berikut daftar kerja kita :</div>
    <ul>
      <li v-for="todo in todos" :key="todo.id">
        {{ todo.task }} <button @click="deleteTodo(todo.id)">-</button>
      </li>
    </ul>
    <input v-model="task" type="text" />
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
    };
  },
  mounted: function () {
    this.getTodos();
  },
  methods: {
    getTodos() {
      axios.get("http://localhost:3000/todo").then((result) => {
        this.todos = result.data;
        console.log(result);
      });
    },
    addTodos() {
      let addItem = { task: this.task };
      console.log(addItem);
      axios.post("http://localhost:3000/todo", addItem);
      this.todos.push(addItem);
    },
    deleteTodo(id) {
      axios.delete(`http://localhost:3000/todo/${id}`).then(() => {
        this.getTodos();
      });
    },
  },
};
</script>