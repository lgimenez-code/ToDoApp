<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        What's up, <input type="text" placeholder="Name here..." v-model="nameUser" />
      </h2>
    </section>
    
    <section class="create-task">
      <h2>Create a task</h2>
      <br>
      <form @submit.prevent="addTask">
        <h4>What's on your task list?</h4>
        <input
          type="text"
          placeholder="e.g. make a video"
          v-model="nameTask"
        />
        <CategorySelector
          :categories="['Business', 'Personal']"
          v-model="inputCategory"
        />
        <input type="submit" value="Add task" />
      </form>
    </section>

    <section class="task-list">
      <div class="list">
        <TaskItem
          :listTasks="orderedTasks"
          @removeTask="removeTask"
        />
      </div>
    </section>
  </main>
</template>

<script setup>
import { ref, onMounted, computed, watch } from "vue";
import TaskItem from './TaskItem.vue'
import CategorySelector from './CategorySelector.vue'
// @update:modelValue="inputCategory = $event"
const arrayTask = ref([]);
const nameUser = ref('');

const nameTask = ref('');
const inputCategory = ref(null);

const orderedTasks = computed(() =>
  arrayTask.value.sort((a, b) => {
    return a.createdAt - b.createdAt;
  })
);

const addTask = () => {
  if (nameTask.value.trim() == '' || inputCategory.value == '') {
    return;
  }
  console.log(inputCategory.value);
  arrayTask.value.push({
    name: nameTask.value,
    category: inputCategory.value,
    done: false,
    createdAt: Date.now(),
  });

  nameTask.value = '';
  inputCategory.value = '';
};

const removeTask = (task) => {
  arrayTask.value = arrayTask.value.filter((t) => t !== task);
}

watch(arrayTask, (newVal) => {
  localStorage.setItem("tasks", JSON.stringify(newVal));
}, { deep:true })

watch(nameUser, (newVal) => {
  localStorage.setItem("name", newVal);
});

onMounted(() => {
  nameUser.value = localStorage.getItem("name") || "";
  arrayTask.value = JSON.parse(localStorage.getItem('tasks') || [])
});
</script>