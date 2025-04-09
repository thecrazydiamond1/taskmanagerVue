<template>
    <div id="task-manager" class="container">
      <h2 class="title">Task Manager </h2>
      <p>{{ newTask }}</p>
      <!-- Add Task Form -->
      <form @submit.prevent="addTask" class="task-form">
        <input v-model="newTask" placeholder="Enter task" class="task-input" />
        <button type="submit" class="add-btn">Add Task</button>
      </form>
  
      <!-- Toggle Button to Show/Hide Task List -->
      <button @click="toggleTaskVisibility" class="toggle-btn">
        {{ showTasks ? 'Hide Tasks' : 'Show Tasks' }}
      </button>
  
      <!-- Task Filters -->
      <div v-show="tasks.length > 0 && showTasks" class="task-filters">
        <button @click="filterTasks('all')" class="filter-btn">All</button>
        <button @click="filterTasks('completed')" class="filter-btn">Completed</button>
        <button @click="filterTasks('pending')" class="filter-btn">Pending</button>
      </div>
  
      <!-- Task List -->
      <ul v-show="tasks.length > 0 && showTasks" class="task-list">
        <li v-for="(task, index) in filteredTasks" :key="index" class="task-item">
          <span :class="{ completed: task.completed }" class="task-name">{{ task.name }}</span>
          <button @click="removeTask(index)" class="delete-btn">❌</button>
          <button @click="toggleTaskStatus(index)" class="status-btn">
            {{ task.completed ? 'Mark as Pending' : 'Mark as Completed' }}
          </button>
        </li>
      </ul>
  
      <!-- Show message when there are no tasks -->
      <p v-show="tasks.length === 0 && showTasks" class="no-tasks-msg"> No tasks available. Add a task! </p>
    </div>
</template>

<script>
export default {
  name: 'TaskManager',
  data() {
    return {
      newTask: '',
      tasks: [],
      filter: 'all',
      showTasks: true,
    };
  },
  computed: {
    filteredTasks() {
      if (this.filter === 'completed') {
        return this.tasks.filter(task => task.completed);
      } else if (this.filter === 'pending') {
        return this.tasks.filter(task => !task.completed);
      }
      return this.tasks;
    }
  },
  methods: {
    addTask() {
      if (this.newTask.trim()) {
        this.tasks.push({ name: this.newTask, completed: false });
        this.newTask = '';
      }
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
    },
    toggleTaskStatus(index) {
      this.tasks[index].completed = !this.tasks[index].completed;
    },
    filterTasks(status) {
      this.filter = status;
    },
    toggleTaskVisibility() {
      this.showTasks = !this.showTasks;
    }
  }
};
</script>

<style scoped>

.container {
  width: 80%;
  margin: auto;
  background: linear-gradient(45deg, #574940, #76bff0);
  padding: 40px;
  border-radius: 15px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  text-align: center;
  font-family: 'Arial', sans-serif;
}

.title {
  font-size: 2.2rem;
  color: #fff;
  margin-bottom: 20px;
  font-weight: 600;
  text-shadow: 2px 2px 12px rgba(255, 255, 255, 0.5);
}

.task-form {
  margin-bottom: 25px;
}

.task-input {
  padding: 12px;
  font-size: 1rem;
  border-radius: 8px;
  border: 2px solid #fe9f59;
  width: 300px;
  margin-right: 12px;
  outline: none;
  transition: border-color 0.3s ease;
}

.task-input:focus {
  border-color: #fdc77b;
}

.add-btn {
  background-color: #fe9f59;
  color: white;
  border: none;
  padding: 12px 22px;
  border-radius: 8px;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.add-btn:hover {
  background-color: #fdc77b;
  transform: scale(1.05);
}

.toggle-btn {
  background-color: #fdc77b;
  color: #333;
  padding: 12px 22px;
  border-radius: 8px;
  font-size: 1rem;
  margin-top: 20px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.toggle-btn:hover {
  background-color: #f1b032;
}

.task-filters {
  margin-top: 20px;
}

.filter-btn {
  background-color: #fe9f59;
  color: white;
  border: none;
  padding: 8px 16px;
  margin: 5px;
  border-radius: 8px;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.filter-btn:hover {
  background-color: #fdc77b;
}

.task-list {
  list-style-type: none;
  padding: 0;
}

.task-item {
  background-color: #fdc77b;
  padding: 12px;
  margin: 12px 0;
  border-radius: 8px;
  color: #fff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: transform 0.3s ease;
}

.task-item:hover {
  transform: scale(1.03);
}

.task-name {
  font-size: 1.2rem;
  font-weight: 600;
}

.completed {
  text-decoration: line-through;
  color: #aaa;
}

.delete-btn,
.status-btn {
  background-color: #e74c3c;
  color: white;
  border: none;
  padding: 6px 14px;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.delete-btn:hover,
.status-btn:hover {
  background-color: #c0392b;
}

.no-tasks-msg {
  color: #13100d;
  font-size: 1.2rem;
  font-weight: 600;
}
</style>
