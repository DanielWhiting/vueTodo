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
  } else {
    todos.value.push({
      content: input_content.value,
      category: input_category.value,
      done: false,
      createdAt: new Date().getTime()
    })
    input_content.value = ''
    input_category.value = null
  }


};

const removeTodo = todo => {
  todos.value = todos.value.filter(t => t !== todo )
}

//  ddep looks through everything and watches for changes
watch(todos, newVal => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, {deep: true})

// watching to set name
watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

// pulling name from local storage
onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
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

    <section class="todo-list">
      <h3>TODO LIST</h3>
      <div class="list">

        <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">
          <label>
            <input type="checkbox" v-model="todo.done" />
            <span :class="`bubble ${todo.category}`"></span>
          </label>
          <div class="todo-content">
            <input type="text" v-model="todo.content" />
          </div>

          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Delete</button>
          </div>
        </div>

      </div>
    </section>
  </main>
</template>
