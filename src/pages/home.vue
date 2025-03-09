<script setup>
import BackendTest from './../components/BackendTest.vue';
import recorder from './../components/recorder.vue';
import holo2 from '../components/holo2.vue';
</script>

<template>
  <div class="bg-blue-50 min-h-screen p-10">
    <h1 class="text-3xl font-bold mb-10 text-center underline">
      欢迎来到我们的项目！
    </h1>
    <p class="text-lg font-semibold mb-5">录音需要接本地后端</p>
    <recorder />
    <p class="text-lg font-semibold mb-5">样式效果预览</p>
    <holo2 />
    <!-- <BackendTest /> -->
  </div>
</template>
