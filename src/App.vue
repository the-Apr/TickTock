<template>
  <main class="app">

    <section class="greeting">
      <h2 class="title">
        What's up, <input 
        type="text" 
        placeholder="Name here" 
        v-model="name" 
        class="">
      </h2>
    </section>

    <section class="create-todo">
      <h3 class="">CREATE A TODO</h3>

      <form @submit.prevent = "addTodo" class="">
        <h4>What's on your todo list?</h4>
        <input 
        type="text" 
        placeholder="e.g attend to my email" v-model="input_content" 
        class="">

        <h4>Pick a category</h4>

        <div class="options ">
          <label>
            <input 
            type="radio" 
            name="category" 
            value="business"
            v-model="input_category"
            class="">
            <span class="bubble business"></span>
            <div>Business</div>
          </label>

          <label>
            <input 
            type="radio" 
            name="category" 
            value="personal"
            v-model="input_category">
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>

          <input type="submit" value="Add todo" >
        </div>
      </form>
    </section>

    <section class="todo-list">
      <h3>TODO LIST</h3>
      <div class="list">

        <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">

          <label>
            <input type="checkbox" v-model="todo.done">
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

<script>
import { ref, onMounted, computed, watch } from 'vue'

export default {
  setup() {
    const todos = ref([])
    const name = ref('')

    const input_content = ref('')
    const input_category = ref(null)

    const todos_asc = computed(() => todos.value.sort((a,b) => {
      return b.createdAt - a.createdAt
    }))

    const addTodo = () => {
      if (input_content.value.trim() === '' || input_category.value === null) {
        return;
      }

      todos.value.push({
        content: input_content.value,
        category: input_category.value,
        createdAt: new Date().getTime(),
        done: false
      })

      input_content.value = ""
      input_category.value = null
    }

    const removeTodo = todo => {
      todos.value = todos.value.filter(t => t !== todo)
    }

    watch(todos, newVal => {
      localStorage.setItem('todos', JSON.stringify(newVal))
    }, {deep: true})

    watch(name, (newVal) => {
      localStorage.setItem('name', newVal)
    }),



    onMounted(() => {
      name.value = localStorage.getItem('name') || ''
      todos.value = JSON.parse(localStorage.getItem('todos') || [])
    })

    return {name,input_category, input_content, addTodo,todos_asc,removeTodo }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  /* color: #2c3e50; */
  
  /* margin-top: 60px; */

  /* background-color: #d1d4d8; */
  min-height: 100vh;
}

</style>
