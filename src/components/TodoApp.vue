<template>

<div class="w-75 m-auto">
<div class="container">
  <h2 class="text-center mt-5">Todo application</h2>
</div>

 <!-- Input -->
  <div class="d-flex mt-5 mb-3">
    <input type="text"
           placeholder="Enter your task"
           class="form-control"
           v-model="task">
    <button @click="submitTask"
            class="btn btn-warning rounded-0"
            style="white-space: nowrap">Add task</button>
  </div>

<!-- Table -->
  <table class="table" v-if="this.tasks.length">
    <thead>
    <tr>
      <th scope="col">Task</th>
      <th scope="col">Status</th>
      <th scope="col" class="text-center">#</th>
      <th scope="col" class="text-center">#</th>
    </tr>
    </thead>
    <tbody>
    <tr v-for="(task, index) in tasks" :key="index">
      <td>
       <span :class="{'finished': task.status === 'Finished'}">{{task.name}}</span>
    </td>
      <td @click="changedTask(index)" class="pointer"
      :class="{'text-danger': task.status === 'To-do',
      'text-warning': task.status === 'In-progress',
       'text-success': task.status === 'Finished'}"
      >{{ task.status }}</td>
      <td>
        <div class="text-center" @click="editTask(index)">
          <span class="fa fa-pen"></span>
        </div>
      </td>
      <td>
        <div class="text-center" @click="deleteTask(index)">
          <span class="fa fa-trash"></span>
        </div>
      </td>
    </tr>
    </tbody>
  </table>

  <div v-if="this.tasks.length === 0" class="text-center">
    <h3>Add more urgent tasks. Soon!
    </h3>
  </div>
</div>
</template>

<script>
export default {
  name: "to-do app",

  data() {
    return {
      task: "",
      editedTask: null,
      availableStatus: ["To-do", "In-progress", "Finished"],
      tasks: [
          { name: "Task 1", status: "In-progress" },
          { name: "Task 2", status: "In-progress" },
          { name: "Task 3", status: "To-do" },
          { name: "Task 4", status: "Finished" },
          { name: "Task 5", status: "In-progress" }
      ]
    }
  },
  methods: {
    submitTask() {
      if (this.task.length === 0) return;
      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: "to-do"
        })
      } else {
        this.tasks[this.editedTask].name = this.task
        this.editedTask = null
      }
      this.task = ""
    },
    deleteTask(index) {
      this.tasks.splice(index, 1)
    },
    editTask(index) {
      this.task = this.tasks[index].name
      this.editedTask = index
    },
    changedTask(index) {
     let newIndex = this.availableStatus.indexOf(this.tasks[index].status)
      if (++newIndex > 2) newIndex = 0
      this.tasks[index].status = this.availableStatus[newIndex]

    }
  }
}

</script>

<style scoped>
.pointer {
  cursor: pointer;
}
.finished {
  text-decoration: line-through;
}
table {
  border: 1px solid #ccc;
  border-radius: 3px;
}
</style>
