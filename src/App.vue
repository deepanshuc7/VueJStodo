<script setup>
import {ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')
// const input_category = ref(null)

const todos_asc = computed(() => todos.value.sort((a,b) => {
  return a.createdAt - b.createdAt
}))

const addTodo = () => {
  if (input_content.value.trim() === '') {
    return
  }
    todos.value.push({
      content: input_content.value,
      // category: input_category.value,
      done: false,
      editable: false,
      createdAt: new Date().getTime()
})
  
}


const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo)
}

watch(todos, (newVal) => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, { deep: true })

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
</script>

<template>
 
 <main class="app">
  <section class="greeting">
    <h2 class="title">
      Hello, <input type="text" id="name" placeholder="Name here" v-model="name" >
    </h2>
  </section>

  <section class="create-todo">
    <h3>Create A Todo</h3>
    <form id="new-todo-form" @submit.prevent="addTodo">
    <h4>What's on your todo list?</h4>
    <input id="content" type="text" placeholder="eg.: Work on the project" v-model="input_content" >
    
    

    <input class="submitBtn" type="submit" value="Add todo">
</form>
  </section>

  <section class="todo-list">
    <h3>Todo List</h3>
    <div class="list" id="todo-list">
      <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">
      <label >
        <input type="checkbox" v-model="todo.done" />
        <span :class="`bubble ${todo.category == 'business' ? 'business' : 'personal'}`"></span>
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


