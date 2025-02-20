<template>
  <div v-if="categories?.length">
    <h4>Pick a category</h4>
    <div class="options">
      <label v-for="category in categories" :key="category">
        <input
          :value="category"
          v-model="inputCategory"
          type="radio"
          name="category"
        />
        <span class="bubble" :class="category.toLowerCase()"></span>
        <div>{{ category }}</div>
      </label>
    </div>
  </div>
</template>

<script setup>
import { ref, defineProps, defineEmits, watch } from "vue";

const props = defineProps({
  categories: {
    type: Array,
    default: () => [],
  },
  modelValue: {
    type: String,
    default: '',
  }
});

const emit = defineEmits(["update:modelValue"]);

const inputCategory = ref(props.modelValue);

watch(() => props.modelValue, (newValue) => {
  inputCategory.value = newValue;
});

watch(inputCategory, (newValue) => {
  emit("update:modelValue", newValue);
});
</script>