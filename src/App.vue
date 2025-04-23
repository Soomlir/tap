<script setup>
import { ref, onMounted, watch } from "vue";

const tasks = ref([]);

async function loadTasks() {
    const savedTasks = localStorage.getItem("tasks");

    if (savedTasks) {
        tasks.value = JSON.parse(savedTasks);
    } else {
        const request = await fetch('/tasks.json');
        const data = await request.json();
        tasks.value = data;
        localStorage.setItem("tasks", JSON.stringify(data));
    }
}

watch(tasks, (newTasks) => {
    localStorage.setItem("tasks", JSON.stringify(newTasks));
}, { deep: true });

onMounted(() => {
    loadTasks();
});
</script>

<template>
    <ul class="app-list">
        <li class="app-list__item" v-for="item in tasks" :key="item.id">
            <label class="app-list__content">
                <input type="checkbox" v-model="item.done" />
                <span :style="{ textDecoration: item.done ? 'line-through' : 'none' }">
                    {{ item.title }}
                </span>
            </label>
        </li>
    </ul>
</template>


<style scoped>
body {
    font-family: Arial, sans-serif;
    background-color: #f5f5f5;
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    text-align: center;
}

.app-list {
    list-style-type: none;
    padding: 0;
    width: 300px;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    overflow: hidden;
}

.app-list__item {
    padding: 15px;
    border-bottom: 1px solid #e0e0e0;
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.app-list__item:last-child {
    border-bottom: none;
}

.app-list__content {
    font-size: 16px;
    color: #333;
    display: flex;
    align-items: center;
    cursor: pointer;
}

.app-list__content input[type="checkbox"] {
    margin-right: 10px;
    cursor: pointer;
}

.app-list__item:hover {
    background-color: #f0f0f0;
    transition: background-color 0.3s ease;
}
</style>
