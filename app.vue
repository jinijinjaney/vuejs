<template>
  <div class="background-container">
    <div class="background"></div>
  </div>
  <div class="container" :class="{ 'dark-mode': darkMode }">
    <div class="header">
      <h1>TO-DO LIST</h1>
      <div class="options" style="margin-bottom: 20px;">
  <input type="text" v-model="newTask" placeholder="Add a new task..." @keypress.enter="addTask" 
         :style="{ borderColor: darkMode ? '#ffffff' : '#ced4da', borderRadius: '5px', padding: '8px 12px', fontFamily: 'Arial, sans-serif', fontSize: '14px', backgroundColor: darkMode ? '#222' : '#fff', color: '#000' /* Text color always black */ }">
  <button class="btn btn-primary" @click="addTask" style="border-radius: 5px; padding: 8px 16px; font-weight: bold; 
          font-size: 14px; background-color: #007bff; color: #fff;">Add Task</button>
</div>

    </div>
    <ul id="taskList">
      <li v-for="(task, index) in tasks" :key="index" :class="{ completed: task.completed }" 
          :style="{ backgroundColor: darkMode ? '#444' : '#f9f9f9', color: darkMode ? '#eee' : '#000' }">
        <input type="checkbox" v-model="task.completed" @click.stop>
        <span>{{ task.text }}</span>
        <button class="btn btn-danger" @click="deleteTask(index)" style="border-radius: 5px; padding: 6px 12px; font-size: 12px; 
                background-color: #dc3545; color: #fff;">Delete</button>
        <button class="btn btn-warning" @click="openEditTab(index)" style="border-radius: 5px; padding: 6px 12px; font-size: 12px; 
                background-color: #ffc107; color: #000;">Edit</button>
      </li>
    </ul>
    <div class="edit-tab" v-if="editIndex !== -1" :style="{ backgroundColor: darkMode ? '#222' : '#fff', borderColor: darkMode ? '#007bff' : '#ced4da', color: darkMode ? '#eee' : '#000' }">
      <input type="text" v-model="editTaskText" placeholder="Edit task..." 
             :style="{ borderColor: darkMode ? '#ffffff' : '#ced4da', borderRadius: '5px', padding: '8px 12px', 
                       fontFamily: 'Arial, sans-serif', fontSize: '14px', backgroundColor: darkMode ? '#333' : '#fff', 
                       color: '#000' /* Text color always black */ }">
      <button class="btn btn-success" @click="saveEdit" style="border-radius: 5px; padding: 6px 12px; font-size: 12px; 
              background-color: #28a745; color: #fff;">Save</button>
      <button class="btn btn-secondary" @click="cancelEdit" style="border-radius: 5px; padding: 6px 12px; font-size: 12px; 
              background-color: #6c757d; color: #fff;">Cancel</button>
    </div>
    <button class="btn-toggle" @click="toggleDarkMode" style="border-radius: 25px; padding: 8px 16px; font-size: 0.8em; 
            font-weight: bold; text-transform: uppercase; letter-spacing: 1px; background-color: #6c757d; color: #fff;">
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
/* Define dark mode styles */
body.dark-mode {
  background-color: #313030;
  color: #dee8f0;
}

/* Background container styles */
.background {
  background-image: url('image/violet.jpg');
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
  max-width: 1200px; /* Adjust the maximum width as per your requirement */
  margin: 50px auto;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.3); /* Increased shadow for more visibility */
}

/* Dark mode specific container styles */
.container.dark-mode {
  background-color: #333333;
  color: #ffffff;
}

/* Dark mode toggle button styles */
.btn-toggle {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background-color: #6c757d;
  color: #ffffff;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

/* Dark mode specific toggle button styles */
.btn-toggle:hover {
  background-color: #5a6268;
}

/* Dark mode styles for input and button elements */
input[type="text"],
.btn {
  color: #000 !important; /* Text color always black */
}

/* Dark mode styles for button text */
.btn {
  color: #fff !important; /* Button text color white */
}

/* Dark mode styles for list items */
ul {
  list-style-type: none; /* Remove bullet points */
  padding: 0; /* Remove default padding */
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

/* Dark mode specific styles for completed tasks */
ul li.completed {
  text-decoration: line-through;
  opacity: 0.6;
}

/* Dark mode specific styles for edit tab */
.edit-tab {
  background-color: #fff;
  border: 2px solid #007bff;
  color: #000000;
  margin-top: 10px;
  padding: 10px;
  border-radius: 5px;
}

/* Dark mode specific styles for buttons inside the edit tab */
.edit-tab .btn {
  background-color: #6c757d;
  color: #ffffff;
}

/* Dark mode specific hover styles for buttons inside the edit tab */
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
  margin-bottom: 20px; /* Adjust margin for prominence */
  font-size: 70px; /* Increase font size for dominance */
  font-weight: bold; /* Ensure it stands out */
  font-family: 'Copperplate', fantasy; /* Choose a bold font */
  color: #4b4b4a; /* Ensure it contrasts well with background */
  -webkit-text-stroke: 0.99px rgb(255, 255, 255); /* Add black stroke around text */
  text-stroke: 0.15px rgb(92, 91, 89); /* Add black stroke around text */
}
</style>
