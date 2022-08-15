<script setup>
  import { ref, onMounted, computed, watch } from 'vue'

  const todos = ref([])
  const name = ref('')

  const inputContent = ref('')
  const inputCategory = ref(null)

  const todosAsc = computed(() => todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt
  }))  

  const addTodo = () => {
    if(inputContent.value.trim() === '' || inputCategory.value === null) {
      return
    }

    todos.value.push({
      content: inputContent.value,
      category: inputCategory.value,
      done: false,
      createdAt: new Date().getTime()
    })

    inputContent.value = ''
    inputCategory.value = null
  }

  const removeTodo = (todo) => {
    todos.value = todos.value.filter(t => t !== todo)
  }
  
  watch(name, (setName) => {
    localStorage.setItem('name', setName)
  })

  watch(todos, (setTodos) => {
    localStorage.setItem('todos', JSON.stringify(setTodos))
  }, { deep: true })

  onMounted(function() {
    name.value = localStorage.getItem('name') || ''
    todos.value = JSON.parse(localStorage.getItem('todos')) || []
  })
</script>

<template>
  <div class="w-screen h-screen bg-gray-100">
    <div class="p-4 max-w-xl mx-auto">
      <h1 class="text-gray-800 text-2xl font-bold my-4">What's up, <input class="bg-transparent outline-none text-sky-600 border-0 focus:ring-0 p-0 text-2xl transition-all ease-out" type="text" placeholder="Your Name" v-model="name"></h1>
    
      <h2 class="text-gray-500 mt-8">CREATE A TODO</h2>
      <form @submit.prevent="addTodo()">
        <div class="flex flex-col mt-4 space-y-2">
          <h3 class="text-gray-800">What's in your to do list?</h3>
          <input name="inputContent" class="px-6 py-3 border-0 outline-none focus:ring-2 focus:ring-sky-600 shadow rounded text-gray-800" type="text" v-model="inputContent">
        </div>
        
        <div class="flex flex-col mt-4 space-y-2">
          <h3 class="text-gray-800">Pick a category</h3>
          <div class="grid grid-cols-2 gap-4">
            <div class="w-full flex flex-col items-center justify-center space-y-2 shadow bg-white h-24 rounded">
              <input name="inputCategory" class="text-sky-600 w-6 h-6 focus:ring-sky-600" type="radio" v-model="inputCategory" value="Business">
              <span class="text-gray-800 text-sm">Business</span>
            </div>

            <div class="w-full flex flex-col items-center justify-center space-y-2 shadow bg-white h-24 rounded">
              <input name="inputCategory" class="text-pink-600 w-6 h-6 focus:ring-pink-600" type="radio" v-model="inputCategory" value="Personal">
              <span class="text-gray-800 text-sm">Personal</span>
            </div>
          </div>
        </div>

        <button class="w-full uppercase font-bold text-white bg-sky-600 px-6 py-3 rounded shadow mt-4 hover:bg-sky-500 transition-all ease-out" type="submit"><i class="fa fa-plus"></i> Add Todo</button>
      </form>

      <h2 class="text-gray-500 mt-8 mb-4">TODO LIST</h2>
      <div v-if="todos.length == 0" class="bg-green-100 ring-2 ring-green-300 rounded px-6 py-3 text-green-300 text-center">
        <h3 ><i class="fa fa-check"></i> Your to do list is empty</h3>
      </div>
      <div v-else  class="space-y-4">
        <div v-for="todo in todosAsc">
          <div :class="`px-6 py-3 bg-white rounded flex items-center justify-between shadow ring-2 ${todo.category == 'Business' ? 'ring-sky-600' : 'ring-pink-600'} ${todo.done ? 'ring-0' : ''}`">
            <div>
              <input type="checkbox" :class="`form-checkbox ${todo.category == 'Business' ? 'text-sky-600 focus:ring-sky-600' : 'text-pink-600 focus:ring-pink-600'}`" name="done" v-model="todo.done">
              <input type="text" :class="`p-0 border-0 text-gray-800 ml-6 focus:ring-0 ${todo.done ? 'text-gray-500 line-through' : ''}`" v-model="todo.content">
            </div>
            <button class="bg-transparent text-red-600 text-sm hover:text-white ring-red-500 hover:bg-red-600 transition-all ease-out px-4 py-2 rounded hover:shadow font-bold text-white" @click="removeTodo(todo)"><i class="fa fa-trash"></i> Delete</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
