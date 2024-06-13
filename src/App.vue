<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        What's up, 
        <input type="text" placeholder="Name here" v-model="name">
      </h2>
    </section>
    <section class="create-todo">
      <h3>Create a todo</h3>
      <form @submit.prevent="addTodo">
        <h4>
          What's on your todo list?
        </h4>
        <input type="text" placeholder="make a video" v-model="inputContext">
        <h4>Pick a category</h4>

        <div class="options">
          <label>
            <input type="radio" name="category" id="category1" value="business" v-model="inputCategory">
            <span class="bubble business"></span>
            <div>Business</div>
          </label>
          <label>
            <input type="radio" name="category" id="category1" value="personal" v-model="inputCategory">
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
        <div v-for="todo in todos" :class="`todo-item ${todo.done && 'done'}`">
            <label>
              <input type="checkbox" v-model="todo.done" />
              <span :class="`bubble ${todo.category}`"></span>
            </label>

            <div class="todo-content">
              <input type="text" v-model="todo.content">
            </div>

            <div class="actions">
              <button class="delete" @click="removeTodo(todo)">Delete</button>
            </div>
        </div>
      </div>
    </section>
  </main>
</template>
<script setup>
import {ref, computed, watch, onMounted} from 'vue'

const todos = ref([]);
const name = ref('');
const inputContext = ref('');
const inputCategory = ref(null);

const todosAsc = computed(()=>{
  todos.value.sort((a,b)=>{
    return b.createAt - a.createAt
  })
});

watch(name, (newVal)=>{
  localStorage.setItem('name',newVal)
});

watch(todos,newVal => {
  localStorage.setItem('todos',JSON.stringify(newVal))
}, {deep:true})

onMounted(()=>{
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
});

const addTodo = () =>{
    if(inputContext.value.trim() ==='' ||inputCategory.value ===null){
      return
    }

    todos.value.push({
      content: inputContext.value,
      category: inputCategory.value,
      done: false,
      createAt: new Date().getTime()
    })

    inputContext.value = ''
    inputCategory.value = null
}

const removeTodo = todo => {
  todos.value = todos.value.filter(t => t !==todo)
}
</script>
