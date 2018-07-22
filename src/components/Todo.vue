<template>
  <div class="todo">
    <h1>Todo</h1>

    <form @submit.prevent="addTask">
      <input type="text" v-model="inputText" />
      <input type="submit" value="add" />
    </form>

    <ul>
      <div v-for="task in tasks" :key="task.id">
        <input type="checkbox" :id="task.id" :name="task.name" :checked="task.checked" @change="updateChecked" />
        <label :for="task.id">{{ task.name }}</label>
      </div>
    </ul>
  </div>
</template>

<script>
import uuidv4 from "uuid/v4"

export default {
  name: "Todo",
  data() {
    return {
      tasks: [],
      inputText: ""
    }
  },
  created: function() {
    if (!localStorage.getItem("todo")) {
      localStorage.setItem("todo", JSON.stringify({ tasks: [] }))
    }

    this.tasks = JSON.parse(localStorage.getItem("todo")).tasks
  },
  methods: {
    addTask: function() {
      if (!this.inputText.trim()) {
        return
      }

      const tasks = JSON.parse(localStorage.getItem("todo")).tasks
      const newTask = { id: uuidv4(), name: this.inputText, checked: false }
      const updatedTasks = tasks.concat(newTask)

      localStorage.setItem("todo", JSON.stringify({ tasks: updatedTasks }))

      this.tasks = updatedTasks
      this.inputText = ""
    },

    updateChecked: function(e) {
      const tasks = JSON.parse(localStorage.getItem("todo")).tasks
      const updatedTasks = tasks.map((task) => {
        if (task.id === e.target.id) {
          task.checked = e.target.checked
        }

        return task
      })

      localStorage.setItem("todo", JSON.stringify({ tasks: updatedTasks }))

      this.tasks = updatedTasks
    }
  }
}
</script>

<style scoped>
.todo {
  margin: auto;
  max-width: 1024px;
  padding: 0 120px;
  text-align: left;
}
</style>
