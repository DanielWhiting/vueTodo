<script setup>
// ref is how we handle state, onMounted render the component local storage, computed to have things in order ORDERED by date, watch will watch for changes and update local storage.
import { ref, onMounted, computed, watch } from "vue";

const todos = ref([]);
const name = ref("");

const input_content = ref("");
const input_category = ref(null);

// sorting todos in ascending order, using computed to access local storage.
const todos_asc = computed(() =>
  todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  })
);

const addTodo = () => {
  if(input_content.value.trim() === ''){
    alert("Cannot have empty todo")
    return
  } 

  if( input_category.value === null) {
    alert("Please select a category")
  }
};

// watching to set name
watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

// pulling name from local storage
onMounted(() => {
  name.value = localStorage.getItem("name") || "";
});
</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        What's up, <input type="text" placeholder="Name Here" v-model="name" />
      </h2>
    </section>

    <section class="create-todo">
      <h3>Create A Todo</h3>
      <form @submit.prevent="addTodo">
        <h4>What's on your todo list?</h4>
        <input
          type="text"
          placeholder="e.g. walk the dog"
          v-model="input_content"
        />
        <h4>Pick a category</h4>
        <div class="options">
          <!-- business category -->
          <label>
            <input
              type="radio"
              name="category"
              id="category1"
              value="business"
              v-model="input_category"
            />
            <span class="bubble business"></span>
            <div>Business</div>
          </label>

          <!-- personal category -->
          <label>
            <input
              type="radio"
              name="category"
              id="category2"
              value="personal"
              v-model="input_category"
            />
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>

        <input type="submit" value="Add todo">
      </form>
    </section>
  </main>
</template>
