<script setup>
import { ref } from 'vue'

defineProps({
  msg: String,
})

const count = ref(0)
</script>

<template>
  <!-- <h1>{{ msg }}</h1> -->

  <div class="card">
    <button type="button" @click="count++" 
    class="border rounded px-2 py-1 cursor-pointer bg-yellow-100 hover:bg-yellow-300"
    >count is {{ count }}</button>
  </div>

</template>
