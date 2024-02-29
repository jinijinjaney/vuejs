<template>
  <div class="background-container">
    <div class="background"></div>
  </div>
  <div class="container" :class="{ 'dark-mode': darkMode }">
    <div class="header">
      <h1>TO-DO LIST</h1>
      <div class="options" style="margin-bottom: 20px;">
        <input type="text" v-model="newTask" placeholder="Type your task" @keypress.enter="addTask" 
               :style="{ borderColor: darkMode ? '#ffffff' : '#ced4da', borderRadius: '5px', padding: '8px 12px', fontFamily: 'Arial, sans-serif', fontSize: '14px', backgroundColor: darkMode ? '#222' : '#fff', color: '#000' }">
        <button class="btn btn-primary" @click="addTask" style="border-radius: 5px; padding: 8px 16px; font-weight: bold; font-size: 14px; background-color: #007bff; color: #fff;">Add Task</button>
      </div>
    </div>
    <ul id="taskList">
    <li v-for="(task, index) in tasks" :key="index" :class="{ completed: task.completed }" 
        :style="{ backgroundColor: darkMode ? '#444' : '#f9f9f9', color: darkMode ? '#eee' : '#000', display: 'flex', alignItems: 'center', justifyContent: 'space-between', padding: '10px', marginBottom: '5px', borderRadius: '5px' }">
      <div style="display: flex; align-items: center;">
        <input type="checkbox" v-model="task.completed" @click.stop>
        <!-- Adjusting space here -->
        <span style="margin-left: 10px;">{{ task.text }}</span>
      </div>
      <div>
        <button class="btn btn-warning" @click="openEditTab(index)" style="border-radius: 5px; padding: 6px 12px; font-size: 12px; background-color: #ffc107; color: #000;">Edit</button>
        <button class="btn btn-danger" @click="deleteTask(index)" style="border-radius: 5px; padding: 6px 12px; font-size: 12px; background-color: #dc3545; color: #fff;">Delete</button>
      </div>
    </li>
  </ul>
    <div v-if="editIndex !== -1" class="edit-tab-overlay">
      <div class="edit-tab" :style="{ backgroundColor: darkMode ? '#222' : '#fff', borderColor: darkMode ? '#007bff' : '#ced4da', color: darkMode ? '#eee' : '#000' }">
        <input type="text" v-model="editTaskText" placeholder="Edit task..." 
               :style="{ borderColor: darkMode ? '#ffffff' : '#ced4da', borderRadius: '5px', padding: '8px 12px', fontFamily: 'Arial, sans-serif', fontSize: '14px', backgroundColor: darkMode ? '#333' : '#fff', color: '#000'  }">
        <button class="btn btn-success" @click="saveEdit" style="border-radius: 5px; padding: 6px 12px; font-size: 12px; background-color: #28a745; color: #fff;">Save</button>
        <button class="btn btn-secondary" @click="cancelEdit" style="border-radius: 5px; padding: 6px 12px; font-size: 12px; background-color: #6c757d; color: #fff;">Cancel</button>
      </div>
    </div>
    <button class="btn-toggle" @click="toggleDarkMode" style="border-radius: 25px; padding: 8px 16px; font-size: 0.8em; font-weight: bold; text-transform: uppercase; letter-spacing: 1px; background-color: #6c757d; color: #fff;">
      <i class="fas fa-adjust"></i> {{ darkMode ? 'Light Mode' : 'Dark Mode' }}
    </button>
  </div>
</template>


<script>
export default {
  data() {
    return {
      tasks: [],
      newTask: '',
      editIndex: -1,
      darkMode: false
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim() === '') {
        alert('Please enter a task!');
        return;
      }
      this.tasks.push({ text: this.newTask, completed: false });
      this.newTask = '';
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    openEditTab(index) {
  if (this.tasks[index].completed) {
    return; // Do not allow editing completed tasks
  }
  this.editIndex = index;
},
    toggleCompleted(index) {
      this.tasks[index].completed = !this.tasks[index].completed;
    },
    saveEdit() {
      if (this.editTaskText.trim() === '') {
        alert('Task cannot be empty!');
        return;
      }
      this.tasks[this.editIndex].text = this.editTaskText.trim();
      this.editIndex = -1;
    },
    cancelEdit() {
      this.editIndex = -1;
    },
    toggleDarkMode() {
      this.darkMode = !this.darkMode;
    }
  },
  computed: {
    editTaskText: {
      get() {
        return this.editIndex !== -1 ? this.tasks[this.editIndex].text : '';
      },
      set(value) {
        this.tasks[this.editIndex].text = value;
      }
    }
  }
};
</script>

<style scoped>

body.dark-mode {
  background-color: #313030;
  color: #dee8f0;
}

.background {
  background-image: url('image/color.jpg');
  background-size: cover;
  background-position: center;
  width: 100%;
  height: 100%;
  filter: blur(5px);
}

.background-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
}

.container {
  position: relative;
  max-width: 1200px;
  margin: 50px auto;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.3);
}


.container.dark-mode {
  background-color: #333333;
  color: #ffffff;
}


.btn-toggle {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background-color: #6c757d;
  color: #ffffff;
  cursor: pointer;
  transition: background-color 0.3s ease;
}


.btn-toggle:hover {
  background-color: #5a6268;
}


input[type="text"],
.btn {
  color: #000 !important;
}


.btn {
  color: #fff !important;
}


ul {
  list-style-type: none;
  padding: 0;
}

ul li {
  background-color: #f9f9f9;
  color: #000000;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
  margin-bottom: 5px;
  border-radius: 5px;
}

ul li .btn-warning,
ul li .btn-danger {
  margin-left: 5px; /* Adjust the space between edit and delete buttons */
}

ul li.completed {
  opacity: 0.6;
}

ul li.completed .btn-danger {
  justify-content: center;
}

ul li.completed span {
  text-decoration: none; /* Remove text decoration */
}

ul li.completed {
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.6); /* Add dark shadow to the container */
}


.edit-tab {
  background-color: #fff;
  border: 2px solid #007bff;
  color: #000000;
  margin-top: 10px;
  padding: 10px;
  border-radius: 5px;
}


.edit-tab .btn {
  background-color: #6c757d;
  color: #ffffff;
}


.edit-tab .btn:hover {
  background-color: #5a6268;
}

.header {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.header h1 {
  margin-bottom: 20px;
  font-size: 70px;
  font-weight: bold;
  font-family: 'Impact', fantasy;
  color: #965000;
  -webkit-text-stroke: 0.99px rgb(255, 255, 255);
  stroke: 0.15px rgb(92, 91, 89);
}

.edit-tab-overlay {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 1000; /* Ensure it's above other elements */
}

.edit-tab {
  background-color: #fff;
  border: 2px solid #0760be;
  color: #000000;
  margin-top: 10px;
  padding: 20px; /* Increase padding for larger size */
  border-radius: 15px; /* Increase border radius for a smoother look */
}

.edit-tab input[type="text"] {
  margin-bottom: 10px; /* Add margin to the input field */
  padding: 12px; /* Increase padding for the input field */
  font-size: 16px; /* Increase font size for better visibility */
}

.edit-tab .btn {
  background-color: #6c757d;
  color: #585858;
  padding: 12px 24px; /* Increase padding for larger buttons */
  font-size: 16px; /* Increase font size for larger buttons */
}

.edit-tab .btn:hover {
  background-color: #5a6268;
}
</style>
