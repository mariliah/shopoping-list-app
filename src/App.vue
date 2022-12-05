<script setup>
import { ref, computed } from 'vue'



const header = ref('Shopping List App')

const editing = ref(false)
const items = ref([
  {
    id: 1,
    label: '10 Party hats',
    purchased: true,
    highPriority: true
  },
  {
    id: 2,
    label: '2 board games',
    purchased: true,
    highPriority: false
  },
  {
    id: 3,
    label: '20 cups',
    purchased: false,
    highPriority: true
  },
  {
    id: 4,
    label: '30 plates',
    purchased: true,
    highPriority: false
  }
])
//important to RETURN computed value from computed function
const reversedItems = computed(() => {
  return [...items.value].reverse()//reversed array
})
const newItem = ref("")
const newItemHighPriority = ref(false)

const saveItem = () => {
  items.value.push(
    {
      id: items.value.length + 1,
      label: newItem.value,
      highPriority: newItemHighPriority.value
    })
  newItem.value = ""
  newItemHighPriority.value = ""
}

const doEdit = (e) => {
  editing.value = e
  newItem.value = ""
  newItemHighPriority.value = ""
}

const togglePurchased = (item) => {
  item.purchased = !item.purchased
}
</script>

<template>
  <div class="header">
    <h1>{{ header }}</h1>
  </div>
  <button v-if="editing" class="btn" @click="doEdit(false)">
    Cancel
  </button>
  <button v-else class="btn btn-primary" @click="doEdit(true)">
    Add item
  </button>
  <form class="add-item-form" @submit.prevent="saveItem" v-if="editing">
    <input type="text" placeholder="Add an item" v-model.trim="newItem">
    Priority:
    <label>
      <input type="checkbox" v-model="newItemHighPriority"> High Priority
    </label>
    <button class="btn btn-primary" :disabled="newItem.length === 0">
      Save Item
    </button>
  </form>
  <ul>
    <li v-for="(item, index) in reversedItems" :key="item.id" class="static-class"
      :class="{ strikeout: item.purchased, priority: item.highPriority }" @click="togglePurchased(items[index])">
      {{ item.label }}
    </li>
  </ul>

  <p v-if="!items.length">
    Nothing to see here
  </p>
</template>