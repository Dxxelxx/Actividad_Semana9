<template>
  <div class="task-manager">
    <h1>Gestor de Tareas</h1>

    <!-- Input y botón para agregar tareas -->
    <div class="input-section">
      <input
        v-model="newTask"
        @keyup.enter="addTask"
        placeholder="Escribe el nombre de la tarea"
      />
      <button @click="addTask">Agregar</button>
    </div>

    <!-- Listado de tareas -->
    <h2>Listado de tareas</h2>

    <div v-if="noTasks" class="no-tasks">
      No hay tareas registradas.
    </div>

    <div v-else class="boards">
      <!-- To Do -->
      <div class="column todo">
        <h3>To Do</h3>
        <div
          v-for="(task, index) in tasks.todo"
          :key="'todo-' + index"
          class="task"
        >
          {{ task }}
          <button @click="moveTask('todo', 'doing', index)">➡</button>
        </div>
      </div>

      <!-- Doing -->
      <div class="column doing">
        <h3>Doing</h3>
        <div
          v-for="(task, index) in tasks.doing"
          :key="'doing-' + index"
          class="task"
        >
          {{ task }}
          <button @click="moveTask('doing', 'done', index)">➡</button>
        </div>
      </div>

      <!-- Done -->
      <div class="column done">
        <h3>Done</h3>
        <div
          v-for="(task, index) in tasks.done"
          :key="'done-' + index"
          class="task"
        >
          {{ task }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const newTask = ref("");
const tasks = ref({
  todo: [],
  doing: [],
  done: [],
});

// Computed: no hay tareas
const noTasks = computed(() => {
  return (
    tasks.value.todo.length === 0 &&
    tasks.value.doing.length === 0 &&
    tasks.value.done.length === 0
  );
});

// Agregar tarea
function addTask() {
  const name = newTask.value.trim();
  if (name === "") return;
  tasks.value.todo.push(name);
  newTask.value = "";
}

// Mover tarea entre secciones
function moveTask(from, to, index) {
  const moved = tasks.value[from][index];
  tasks.value[from].splice(index, 1);
  tasks.value[to].push(moved);
}
</script>

<style scoped>
.task-manager {
  text-align: center;
  font-family: Arial, sans-serif;
  padding: 20px;
}

.input-section {
  margin-bottom: 20px;
}

.input-section input {
  padding: 8px;
  width: 250px;
  border-radius: 5px;
  border: 1px solid #ccc;
}

.input-section button {
  padding: 8px 15px;
  margin-left: 10px;
  background-color: #007bff;
  border: none;
  border-radius: 5px;
  color: white;
  cursor: pointer;
}

.input-section button:hover {
  background-color: #0056b3;
}

.boards {
  display: flex;
  justify-content: space-around;
  margin-top: 20px;
}

.column {
  width: 30%;
  padding: 15px;
  border: 2px dashed #ccc;
  border-radius: 10px;
  min-height: 200px;
}

.column h3 {
  margin-bottom: 10px;
}

.todo {
  background-color: #e7f0ff;
}

.doing {
  background-color: #e9ffe7;
}

.done {
  background-color: #ffe7e7;
}

.task {
  background-color: white;
  border-radius: 5px;
  margin: 8px 0;
  padding: 8px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
}

.task button {
  background: transparent;
  border: none;
  font-size: 18px;
  cursor: pointer;
  color: #555;
}

.no-tasks {
  margin-top: 15px;
  font-style: italic;
  color: gray;
}
</style>

