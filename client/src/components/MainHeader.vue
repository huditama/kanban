<template>
  <div class="main-header">
    <h1>Kan-Han™ by Reyhan Huditama!</h1>
    <input
      type="text"
      placeholder="Create a task!!!"
      v-model="taskName"
      style="border:3px solid black"
    >
    <input
      type="submit"
      @click="createTask"
      style="width:5%;color:white;background-color:rgb(69,134,239);"
    >
  </div>
</template>

<script>
import database from "../assets/config.js";
import swal from "sweetalert";

export default {
  name: "MainHeader",
  data() {
    return {
      taskName: ""
    };
  },
  methods: {
    createTask() {
      database.ref("/").push({
        title: this.taskName,
        status: "Backlog"
      });
      swal("Success!", "Created a new task!", "success");
      this.taskName = "";
    }
  }
};
</script>

<style scoped>
html,
body {
  font-family: "Lato";
  margin: 0;
  padding: 0;
  overflow-x: hidden;
}

h1,
h2,
h3 {
  padding: 0;
  margin: 0;
}

.main-header h1 {
  color: #4684f6;
  margin-left: auto;
  margin-left: auto;
  text-align: center;
  border-top: #4684f6 3px solid;
  padding-top: 1.5%;
  font-size: 35px;
}

.main-header i {
  font-size: 20px;
  color: #4684f6;
  text-align: center;
  margin-left: auto;
  margin-right: auto;
  display: block;
  margin-top: 50px;
  cursor: pointer;
}

.main-header input {
  display: block;
  margin-left: auto;
  margin-right: auto;
  text-align: center;
}

input {
  height: 30px;
  width: 300px;
  margin-bottom: 20px;
  margin-top: 20px;
  border-radius: 5px;
}
</style>