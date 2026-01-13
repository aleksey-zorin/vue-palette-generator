<template>
  <div 
    class="color-card"
    :style="{ backgroundColor: color }"
    @click="$emit('click', color)"
  >
    <span class="hex-value">{{ color }}</span>
  </div>
</template>

<script setup>
defineProps({
  color: {
    type: String,
    required: true
  }
})

defineEmits(['click'])
</script>

<style scoped>
.color-card {
  padding: 20px;
  border-radius: 8px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100px;
}

.hex-value {
  background-color: rgba(0, 0, 0, 0.7);
  color: white;
  padding: 5px 10px;
  border-radius: 4px;
  font-family: monospace;
}
</style>