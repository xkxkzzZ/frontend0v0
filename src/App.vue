<script setup>
import HelloWorld from './components/HelloWorld.vue'
import BackendTest from './components/BackendTest.vue';
import SideBar from './components/SideBar.vue';
import Mainright from './components/Mainright.vue';
</script>

<template>
  <!-- <div>
    <a href="https://vite.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div> -->
  <!-- <HelloWorld msg="Vite + Vue" /> -->
  

  <!-- <h1 class="text-3xl font-bold underline">
    欢迎来到我们的项目！
  </h1> -->


  <!-- <BackendTest /> -->

  <div class="flex">
    <SideBar />
    <Mainright />
  </div>
</template>
