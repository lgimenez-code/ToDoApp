<template>
  <div v-if="!listTasks?.length" :class="'task-item'">
    <div class="task-content">No hay tareas</div>
  </div>
  <div v-else>
    <div v-for="task in listTasks" :class="`task-item ${task.done && 'done'}`">
      <label>
        <input type="checkbox" v-model="task.done" />
        <span :class="`bubble ${task.category.toLowerCase()}`"></span>
      </label>
      <div class="task-content">
        <span>{{ task.name }}</span>
      </div>
      <div class="actions">
        <button class="delete" @click="emit('removeTask', task)">Delete</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from "vue";

const props = defineProps({
  listTasks: Array,
  default: () => [],
});

const emit = defineEmits(["removeTask"]);
</script>
