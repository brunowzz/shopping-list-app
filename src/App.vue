<script setup lang="ts">
import { ref } from "vue";

const header = ref<string>("Shopping List App");
const items = ref([
  { id: 1, label: "10 Party hats", priority: false },
  { id: 2, label: "2 board games", priority: false },
  { id: 3, label: "1 bag of chips", priority: false },
]);
const newItem = ref<string>("");
const newItemHighPriority = ref<boolean>(false);

const handleSubmit = () => {
  if (newItem.value === "" || !newItemHighPriority.value) {
    return alert("Please fill out all fields");
  }

  const newItemPush = items.value.push({
    id: items.length + 1,
    label: newItem.value,
    priority: newItemHighPriority.value,
  });

  newItem.value = "";
  newItemHighPriority.value = false;
};
</script>

<template>
  <h1>{{ header }}</h1>

  <form class="add-item-form" @submit.prevent="handleSubmit()">
    <input type="text" placeholder="Add an item" v-model="newItem" />
    <label>
      <input type="checkbox" name="priority" v-model="newItemHighPriority" />
      High Prority
    </label>
    <button class="btn btn-primary" type="submit">Add an item</button>
  </form>

  <ul>
    <li v-for="{ id, label } in items" :key="id">{{ label }}</li>
  </ul>
</template>
