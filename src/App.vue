<template>
  <div class="">
    <h1 class="text-center bg-primary text-white p-3">
      {{ title }}'s To do list
    </h1>
    <div class="container">
      <div class="row my-3">
        <div class="col">
          <input
            type="text"
            class="form-control"
            v-model="newTask"
            v-on:keyup.enter="addTask()"
          />
        </div>
        <div class="col-4 d-flex justify-content-between">
          <input
            type="button"
            class="btn btn-success"
            value="Add"
            @click="addTask"
          />
          <button class="btn btn-danger" @click="deleteTask">Delete</button>
        </div>
      </div>
      <div class="" v-if="filterTask.length > 0">
        <div class="row">
          <div class="col">Tasks</div>
          <div class="col-2">Done</div>
        </div>
        <div class="row" v-for="(task, index) in filterTask" :key="index">
          <div class="col" :class="task.done == true ? 'delete' : ''">
            {{ task.action }}
          </div>
          <div class="col-2">
            <input type="checkbox" name="" v-model="task.done" />
          </div>
        </div>
        <div
          class="bg-info text-white text-center d-flex justify-content-center align-items-center py-2"
        >
          <h5 class="col">Hide Completed Tasks</h5>
          <input
            type="checkbox"
            name=""
            class="col-2 form-check"
            v-model="hideCompleted"
          />
        </div>
      </div>
      <div class="alert alert-warning text-center" v-else>There is no data</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data: () => ({
    title: "ASK",
    hideCompleted: false,
    newTask: "",
    tasks: [],
  }),
  methods: {
    addTask() {
      if (this.newTask === "") {
        return alert("Please add a text!");
      }
      this.tasks.push({
        action: this.newTask,
        done: false,
      });
      this.storeData();
      this.newTask = "";
    },
    deleteTask() {
      this.tasks = this.tasks.filter((v) => !v.done);
      this.storeData();
    },
    storeData() {
      localStorage.setItem("myLocalTasks", JSON.stringify(this.tasks));
    },
  },
  mounted() {
    let data = localStorage.getItem("myLocalTasks");
    if (data !== null) {
      this.tasks = JSON.parse(data);
    }
  },
  computed: {
    filterTask() {
      return this.hideCompleted
        ? this.tasks.filter((v) => !v.done)
        : this.tasks;
    },
  },
};
</script>

<style>
.delete {
  text-decoration: line-through;
}
</style>
