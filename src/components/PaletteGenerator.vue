<template>
  <div class="palette-generator">
    <h2>🎨 Генератор цветовых палитр</h2>
    
    <button @click="generatePalette" class="generate-btn">
      🎲 Случайная палитра
    </button>
    
    <div class="controls">
      <div class="control-group">
        <label>Количество цветов:</label>
        <select v-model="colorsCount">
          <option value="3">3</option>
          <option value="5">5</option>
          <option value="7">7</option>
        </select>
      </div>
      
      <div class="control-group">
        <label>Формат:</label>
        <select v-model="colorFormat">
          <option value="hex">HEX</option>
          <option value="rgb">RGB</option>
        </select>
      </div>
    </div>
    
    <div class="palette-display">
      <div 
        v-for="(color, index) in palette" 
        :key="index"
        class="color-card"
        :style="{ backgroundColor: color.hex }"
        @click="copyToClipboard(color)"
      >
        <div class="color-info">
          <span class="color-index">#{{ index + 1 }}</span>
          <span class="color-value">{{ formatColor(color) }}</span>
        </div>
      </div>
    </div>
    
    <div v-if="notification" class="notification">
      {{ notification }}
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'

const colorsCount = ref(5)
const colorFormat = ref('hex')
const palette = ref([])
const notification = ref('')

// Генерация случайного цвета
const generateRandomColor = () => {
  const letters = '0123456789ABCDEF'
  let color = '#'
  for (let i = 0; i < 6; i++) {
    color += letters[Math.floor(Math.random() * 16)]
  }
  return {
    hex: color,
    rgb: hexToRgb(color)
  }
}

// HEX в RGB
const hexToRgb = (hex) => {
  const result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex)
  return result ? {
    r: parseInt(result[1], 16),
    g: parseInt(result[2], 16),
    b: parseInt(result[3], 16)
  } : { r: 0, g: 0, b: 0 }
}

// Форматирование цвета
const formatColor = (color) => {
  if (colorFormat.value === 'rgb') {
    return `rgb(${color.rgb.r}, ${color.rgb.g}, ${color.rgb.b})`
  }
  return color.hex
}

// Генерация палитры
const generatePalette = () => {
  const newPalette = []
  for (let i = 0; i < colorsCount.value; i++) {
    newPalette.push(generateRandomColor())
  }
  palette.value = newPalette
  saveToLocalStorage()
}

// Копирование в буфер
const copyToClipboard = async (color) => {
  try {
    const text = formatColor(color)
    await navigator.clipboard.writeText(text)
    notification.value = `Скопировано: ${text}`
    setTimeout(() => {
      notification.value = ''
    }, 2000)
  } catch (err) {
    notification.value = 'Ошибка копирования'
  }
}

// Сохранение
const saveToLocalStorage = () => {
  localStorage.setItem('palette', JSON.stringify(palette.value))
}

// Загрузка
const loadFromLocalStorage = () => {
  const saved = localStorage.getItem('palette')
  if (saved) {
    palette.value = JSON.parse(saved)
  } else {
    generatePalette()
  }
}

// Отслеживание изменения количества цветов
watch(colorsCount, () => {
  generatePalette()
})

onMounted(() => {
  loadFromLocalStorage()
})
</script>

<style scoped>
.palette-generator {
  padding: 20px;
  max-width: 1000px;
  margin: 0 auto;
}

.generate-btn {
  padding: 12px 24px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 1.1rem;
  cursor: pointer;
  margin-bottom: 20px;
}

.controls {
  display: flex;
  gap: 30px;
  margin-bottom: 30px;
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 10px;
}

.control-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.control-group label {
  font-weight: bold;
  color: #495057;
}

.control-group select {
  padding: 8px 12px;
  border: 1px solid #ced4da;
  border-radius: 6px;
  background-color: white;
}

.palette-display {
  display: flex;
  height: 200px;
  border-radius: 12px;
  overflow: hidden;
  margin-bottom: 20px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}

.color-card {
  flex: 1;
  display: flex;
  align-items: flex-end;
  padding: 15px;
  cursor: pointer;
  transition: flex 0.3s ease;
  position: relative;
}

.color-card:hover {
  flex: 1.5;
}

.color-info {
  background-color: rgba(0, 0, 0, 0.7);
  color: white;
  padding: 8px 12px;
  border-radius: 6px;
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.color-index {
  font-size: 0.9rem;
  opacity: 0.8;
}

.color-value {
  font-family: monospace;
  font-weight: bold;
}

.notification {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background-color: #28a745;
  color: white;
  padding: 12px 20px;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.2);
  animation: slideIn 0.3s ease;
}

@keyframes slideIn {
  from {
    transform: translateX(100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}
</style>