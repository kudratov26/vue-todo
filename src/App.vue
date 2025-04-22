<template>
  <div class="app">
    <h1>Список задач</h1>
    <ul>
      <li v-for="task in tasks" :key="task.id">
        <label>
          <input type="checkbox" v-model="task.done" @change="saveTasks" />
          <span :style="{ textDecoration: task.done ? 'line-through' : 'none' }">
            {{ task.title }}
          </span>
        </label>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: []
    };
  },
  async created() {
    const saved = localStorage.getItem("tasks");
    if (saved) {
      this.tasks = JSON.parse(saved);
    } else {
      const res = await fetch("/tasks.json");
      const data = await res.json();
      this.tasks = data;
      localStorage.setItem("tasks", JSON.stringify(data));
    }
  },
  methods: {
    saveTasks() {
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    }
  }
};
</script>

<style>
body {
  font-family: sans-serif;
  background: #f5f5f5;
  padding: 2rem;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  background: white;
  margin-bottom: 10px;
  padding: 10px;
  border-radius: 4px;
}
</style>
