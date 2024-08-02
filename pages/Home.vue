<template>
  <div class="bg-[#F6F4F4] h-[full]">
    <nav class="w-[90%] h-[90px] bg-[#09E138] ml-[5%] flex items-center rounded-md">
      <div class="flex space-x-[1%] w-full justify-center items-center">
        <img src="/public/completed-task.png" alt="" width="45px">
        <h1 class="text-[40px] font-[800] text-white">ListCreator</h1>
      </div>
    </nav>
    <div class="flex justify-center mt-[3%]">
      <div class="md:w-[70%] max-[767px]:w-[90%] flex-col justify-center bg-[#FFFFFF] h-[auto] md:pb-[400px] max-[767px]:pb-[100px] drop-shadow-lg border-2 rounded-md">
        <div class="flex justify-center">
          <h2 class="font-[600] mt-[3%] text-[20px] text-[#09E138]">You can add there some tasks</h2>
        </div>
        <div class="flex justify-center space-x-2 md:mt-[2%] max-[767px]:mt-[10%]">
          <input type="text" placeholder="Write your task..." class="border-2 md:w-[45%] max-[767px]:w-[70%] h-[40px] rounded-full pl-[10px]" v-model="newTask.title">
          <button class="bg-[#09E138] w-[60px] h-[40px] rounded-full flex items-center justify-center text-white hover:bg-[#00B33C]" @click="addTask">
            <h1 class="text-lg ">+Add</h1>
          </button>
        </div>
        <div class="flex-col justify-center md:mt-[3%] max-[767px]:mt-[8%]">
          <ul class="space-y-4">
            <li v-for="(task, index) in tasks" :key="index" class="bg-[#000000] text-white p-4 rounded-md flex items-center space-x-4 md:w-[50%] max-[767px]:w-[90%] mx-auto">
              <div v-if="editIndex !== index" class="flex-1 max-w-[65%] text-left break-words overflow-hidden">
                {{ task.title }}
              </div>
              <input v-if="editIndex === index" type="text" v-model="editTask.title" class="flex-1 max-w-[65%] text-left break-words overflow-hidden text-[#000000]">
              <div class="flex items-center space-x-4">
                <img v-if="editIndex !== index" src="/public/pen.png" @click="startEdit(index, task)" alt="Edit" width="40px">
                <img v-if="editIndex === index" src="/public/diskette.png" @click="confirmSaveEdit" alt="Save" width="40px">
                <img src="/public/delete.png" @click="deleteTask(index)" alt="Delete" width="40px">
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
h1 {
  color: white;
}
</style>

<script>
export default {
  data() {
    return {
      newTask: {
        title: ''
      },
      tasks: [],
      editIndex: null,
      editTask: {
        title: ''
      }
    }
  },
  mounted() {
    this.loadTasks();
  },
  methods: {
    addTask() {
      if (this.newTask.title.trim()) {
        this.tasks.push({ title: this.newTask.title.trim() });
        this.saveTasks();
        this.newTask.title = '';
      } else {
        alert("Please enter a task.");
      }
    },
    startEdit(index, task) {
      this.editIndex = index;
      this.editTask = { ...task };
    },
    confirmSaveEdit() {
      if (this.editTask.title.trim()) {
        this.saveEdit();
      } else {
        alert("Please enter a valid task.");
      }
    },
    saveEdit() {
      if (this.editIndex !== null) {
        this.tasks.splice(this.editIndex, 1, this.editTask);
        this.editIndex = null;
        this.editTask = { title: '' };
        this.saveTasks();
      }
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
      this.saveTasks();
    },
    saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },
    loadTasks() {
      const savedTasks = localStorage.getItem('tasks');
      if (savedTasks) {
        this.tasks = JSON.parse(savedTasks);
      }
    }
  }
}
</script>
