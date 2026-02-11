<script setup lang="ts">
import { onMounted, ref } from 'vue'

const STORAGE_KEY = 'mf:quick-note'

const inputValue = ref('')
const savedValue = ref('')
const lastSavedAt = ref('')

const saveValue = () => {
  const value = inputValue.value.trim()
  if (value.length === 0) {
    localStorage.removeItem(STORAGE_KEY)
    savedValue.value = ''
    lastSavedAt.value = ''
    return
  }

  localStorage.setItem(STORAGE_KEY, value)
  savedValue.value = value
  lastSavedAt.value = new Date().toLocaleString()
}

onMounted(() => {
  const stored = localStorage.getItem(STORAGE_KEY)
  if (stored) {
    inputValue.value = stored
    savedValue.value = stored
  }
})
</script>

<template>
  <main class="page">
    <section class="card">
      <header class="header">
        <p class="tag">快速測試</p>
        <h1>製造流程紀錄</h1>
        <p class="subtitle">輸入後按確認，重新開啟仍會顯示。</p>
      </header>

      <div class="form">
        <label class="label" for="quick-input">這次要記的內容</label>
        <input
          id="quick-input"
          v-model="inputValue"
          type="text"
          inputmode="text"
          placeholder="例如：機殼打磨完成，等待烤漆"
          class="input"
          @keyup.enter="saveValue"
        />
        <button class="button" type="button" @click="saveValue">確認並儲存</button>
      </div>

      <div class="result" v-if="savedValue">
        <p class="result-title">已儲存內容</p>
        <p class="result-value">{{ savedValue }}</p>
        <p class="result-time" v-if="lastSavedAt">最後更新：{{ lastSavedAt }}</p>
      </div>
      <p class="hint" v-else>尚未儲存任何內容。</p>
    </section>
  </main>
</template>
