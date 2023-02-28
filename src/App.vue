<script setup>

// ref is how we handle state, onMounted render the component local storage, computed to have things in order ORDERED by date, watch will watch for changes and update local storage. 
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')
const inpute_category = ref(null)

// sorting todos in ascending order, using computed to access local storage.
const todos_asc = computed(() => todos.value.sort((a,b) => {
  return b.createdAt - a.createdAt
}))


// watching to set name 
watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

// pulling name from local storage
onMounted(() => {
  name.value = localStorage.getItem('name') || ''
})

</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">What's up, <input type="text" placeholder="Name Here" v-model="name">
      </h2>
    </section>
  </main>
</template>

