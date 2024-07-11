<script setup lang="ts">
import { ref } from "vue";

const header = ref<string>("Shopping List App");
const editing = ref<boolean>(false);
const items = ref([]);
const newItem = ref<string>("");
const newItemHighPriority = ref<boolean>(false);

const handleSubmit = () => {
  if (newItem.value === "") {
    return alert("Please fill out all fields");
  }

  const newItemPush = items.value.push({
    id: items.length + 1,
    label: newItem.value,
    priority: newItemHighPriority.value,
    purchased: false,
  });

  newItem.value = "";
  newItemHighPriority.value = false;
};

const doEdit = (e: Event) => {
  editing.value = e;
  newItem.value = "";
};

const togglePurchased = (item) => {
  item.purchased = !item.purchased;
};
</script>

<template>
  <div class="header">
    <h1>{{ header }}</h1>
    <button v-if="editing" class="btn" @click="doEdit(false)">Cancel</button>
    <button v-else class="btn btn-primary" @click="doEdit(true)">
      Add Item
    </button>
  </div>

  <form class="add-item-form" v-if="editing" @submit.prevent="handleSubmit()">
    <input type="text" placeholder="Add an item" v-model="newItem" />
    <label>
      <input type="checkbox" name="priority" v-model="newItemHighPriority" />
      High Prority
    </label>
    <button
      :disabled="newItem.length < 3"
      type="submit"
      class="btn btn-primary"
    >
      Add an item
    </button>
  </form>

  <ul>
    <li
      v-for="item in items"
      class="static-class"
      @click="togglePurchased(item)"
      :class="{ strikeout: item.purchased, priority: item.priority }"
      :key="item.id"
    >
      {{ item.label }}
    </li>
  </ul>

  <p v-if="!items.length">Nothing to see here</p>
</template>
