<template>
  <main class="app py-8 px-11 flex flex-col space-y-4">

    <section class="greeting">
      <h2 class="title">
        What's up, <input 
        type="text" 
        placeholder="Name here" 
        v-model="name" 
        class="bg-transparent outline-none">
      </h2>
    </section>

    <section class="create-todo">
      <h3 class="mb-4">CREATE A TODO</h3>

      <form @submit.prevent = "addTodo" class="flex flex-col space-y-6">
        <h4>What's on your todo list?</h4>
        <input 
        type="text" 
        placeholder="e.g attend to my email" v-model="input_content" 
        class="p-4  outline-none">

        <h4>Pick a category</h4>

        <div class="options flex flex-row space-x-8 justify-center">
          <label for="">
            <input 
            type="radio" 
            name="category" 
            value="business"
            v-model="input_category"
            class="">
            <span class="bubble business"></span>
            <div>Business</div>
          </label>

          <label for="">
            <input 
            type="radio" 
            name="category" 
            value="personal"
            v-model="input_category">
            <span class="bubble personal"></span>
            <div>Business</div>
          </label>
        </div>
      </form>
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

    watch(name, (newVal) => {
      localStorage.setItem('name', newVal)
    })

    onMounted(() => {
      name.value = localStorage.getItem('name') || ''
    })

    return {name}
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 60px; */

  background-color: #d1d4d8;
  min-height: 100vh;
}

.app{
  
}
</style>
