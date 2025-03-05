<script setup>
import { ref } from "vue";

const mediaRecorder = ref(null);
const audioChunks = ref([]);
const audioUrl = ref(null);
const isRecording = ref(false);
const isPlaying = ref(false);
const audioFile = ref(null);

const startRecording = async () => {
  if (!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia) {
    alert("您的浏览器不支持音频录制");
    return;
  }

  try {
    const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
    mediaRecorder.value = new MediaRecorder(stream);
    audioChunks.value = [];

    mediaRecorder.value.ondataavailable = (event) => {
      audioChunks.value.push(event.data);
    };

    mediaRecorder.value.onstop = async () => {
      const audioBlob = new Blob(audioChunks.value, { type: "audio/webm" });
      audioUrl.value = URL.createObjectURL(audioBlob);
      audioFile.value = audioBlob;

      // 上传音频
      await uploadAudio(audioBlob);
    };

    mediaRecorder.value.start();
    isRecording.value = true;
  } catch (error) {
    console.error("录音失败:", error);
  }
};

const stopRecording = () => {
  if (mediaRecorder.value) {
    mediaRecorder.value.stop();
    isRecording.value = false;
  }
};

const uploadAudio = async (blob) => {
  const formData = new FormData();
  formData.append("file", blob, "recording.webm");

  try {
    const response = await fetch("http://localhost:8000/upload/", {
      method: "POST",
      body: formData,
    });
    const result = await response.json();
    audioUrl.value = result.audio_url;
  } catch (error) {
    console.error("音频上传失败:", error);
  }
};
</script>

<template>
  <div class="audio-recorder">
    <div class="recording-section">
      <button @click="startRecording" v-if="!isRecording" class="record-btn">
        🎤 开始录音
      </button>
      <button @click="stopRecording" v-if="isRecording" class="stop-btn">
        ⏹ 停止录音
      </button>
    </div>

    <div class="playback-section" v-if="audioUrl">
      <audio :src="audioUrl" controls></audio>
    </div>
  </div>
</template>

<style scoped>
.audio-recorder {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
}

.recording-section {
  margin-bottom: 20px;
}

.record-btn, .stop-btn {
  font-size: 18px;
  padding: 10px 20px;
  border-radius: 8px;
  cursor: pointer;
}

.record-btn {
  background-color: #4caf50;
  color: white;
}

.stop-btn {
  background-color: #f44336;
  color: white;
}
</style>
