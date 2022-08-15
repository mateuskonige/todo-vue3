<script setup>
  import { ref, onMounted, computed, watch } from 'vue'

  const todos = ref([])
  const name = ref('')
  
</script>

<template>
  <div class="w-screen h-screen bg-gray-100">
    <div class="p-4 max-w-xl mx-auto">
      <h1 class="text-gray-900 text-xl font-bold">What's up, <input class="bg-transparent outline-none text-sky-600" type="text" placeholder="Your Name" v-model="name"></h1>
    </div>
  </div>
</template>