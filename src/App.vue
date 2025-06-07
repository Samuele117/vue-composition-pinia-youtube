<script setup>
import { ref,reactive, computed } from "vue";
import Tasks from "./components/Tasks.vue"
import Filter from "./components/Filter.vue"
import Modal from "./components/ModalWindow.vue"

const appName = ref("my new task manager");

const newTask = reactive({
  completed: false,
 })

const filteredTasks = computed(() => {
  if(filterBy.value === 'todo') {
    return tasks.filter(t => t.completed === false)
  } else if (filterBy.value === 'done') {
    return tasks.filter(t => t.completed === true)
  } else {
    return tasks
  }
})

 const tasks= reactive([
    {
      id : 1,
      name: "Website design",
      description: "Define the style guide, branding and create the webdesign on Figma.",
      completed: true
    },
    {
      id: 2,
      name: "Website development",
      description: "Develop the portfolio website using Vue JS.",
      completed: false
    },
    {
      id: 3,
      name: "Hosting and infrastructure",
      description: "Define hosting, domain and infrastructure for the portfolio website.",
      completed: false
    },
    {
      id: 4,
      name: "Composition API",
      description: "Learn how to use the composition API and how it compares to the options API.",
      completed: true
    },
    {
      id: 5,
      name: "Pinia",
      description: "Learn how to setup a store using Pinia.",
      completed: true
    },
    {
      id: 6,
      name: "Groceries",
      description: "Buy rice, apples and potatos.",
      completed: false
    },
    {
      id: 7,
      name: "Bank account",
      description: "Open a bank account for my freelance business.",
      completed: false
    }
]);

 function toggleCompleted (id) {
  tasks.forEach(task => {
    if (task.id == id){
    if (task.completed == true) {
       task.completed = false 
    } else {
       task.completed = true
    }
   }
  })
  // if (tasks.completed == true) {

  // }
 }

const filterBy = ref('')

function addTask(){
  if (newTask.name && newTask.description) {  
    newTask.id = tasks.map(t => t.id).pop()+ 1
    tasks.push({
    id: newTask.id,
    name: newTask.name, 
    description: newTask.description, 
    completed: newTask.completed
  })
  newTask.name = ''
  newTask.description = ''
  modalIsActive.value = false
} else {
  alert('campi vuoti')
}

}

function setFilter (value){
  filterBy.value = value
}

const modalIsActive = ref(false)
</script>

<template>

  <main class="container">
    <div class="header">
      <div class="header-side">
        <h1>
          {{ appName }}
        </h1>
      </div>
      <div class="header-side">
        <button @click="modalIsActive=true" class="btn secondary">+ Add Task</button>
      </div>
    </div>

    <Filter @filter="setFilter" :filterBy="filterBy"></Filter>

    <Tasks @toggle="toggleCompleted" v-for="(task, index) in filteredTasks" :task="task" :key="task.id" class="tasks" />

    <Modal @closePopup="modalIsActive = false" v-if="modalIsActive">
      <div class="form">
          <h3>Add a new task</h3>
          <input v-model="newTask.name" type="text" name="title" placeholder="Enter a title..."><br />
          <textarea v-model="newTask.description" name="description" rows="4"
            placeholder="Enter a description..." /><br />
          <button @click="addTask" class="btn gray">Add Task</button>
      </div>
    </Modal>
  </main>



</template>


<style lang="scss" scoped>

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;

  .header-side {
    display: flex;
    align-items: center;

    h1 {
      text-transform: capitalize;
      font-size: 42px;
      font-weight: 700;
      line-height: 47px;
      letter-spacing: 0em;
      text-align: left;
    }

    .secondary {
      margin-left: 12px;
    }
  }

}


.add-task {
  margin-top: 60px;

  input, textarea {
    width: 360px;
    max-width: 100%;
    margin-top: 12px;
    padding: 5px;
  }

  button {
    width: 360px;
    margin-top: 12px;
  }
}


</style>