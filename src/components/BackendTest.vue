<script setup>
import { ref } from 'vue';

const result = ref('');
const apiUrl = "https://exact-relieved-yak.ngrok-free.app/ping"; // 后端 API 地址

const testConnection = async () => {
  try {
    const response = await fetch(apiUrl, {
      method: "GET",
      headers: {
        "ngrok-skip-browser-warning": "true" // 跳过提示头
      }
    });
    const data = await response.json();
    result.value = "连接成功！" + JSON.stringify(data);
  } catch (error) {
    result.value = "请求失败: " + error;
  }
};
</script>

<template>
  <div class="text-gray-600 text-lg  bg-gray-100/70 p-4 rounded-lg shadow
    w-1/4 fixed left-10 bottom-10">
    <div class="flex p-2 items-center justify-center">
      <button 
        @click="testConnection" 
        class="hover:text-black underline  cursor-pointer px-1"
      >测试</button>
      <p class=""> 与服务器后端的连接~</p>
      
    </div>
    <p class="text-sm text-gray-800 text-center bg-white leading-8 rounded">{{ result }}</p>
  </div>
</template>