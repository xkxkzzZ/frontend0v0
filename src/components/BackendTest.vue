<script setup>
import { ref } from 'vue';

const result = ref('');
const apiUrl = "https://exact-relieved-yak.ngrok-free.app/ping"; // 你的后端 API 地址

const testConnection = async () => {
  try {
    const response = await fetch(apiUrl, {
      method: "GET",
      headers: {
        "ngrok-skip-browser-warning": "true" // 关键代码，避免 ngrok 提示
      }
    });
    const data = await response.json();
    result.value = "服务器返回: " + JSON.stringify(data);
  } catch (error) {
    result.value = "请求失败: " + error;
  }
};
</script>

<template>
  <div>
    <h1>FastAPI 连接测试</h1>
    <button @click="testConnection">测试连接</button>
    <p>{{ result }}</p>
  </div>
</template>

<style scoped>
h1 {
  color: #42b983;
}
button {
  background-color: #42b983;
  color: white;
  padding: 10px 15px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
}
button:hover {
  background-color: #369973;
}
p {
  font-size: 1.2em;
  color: #333;
}
</style>
