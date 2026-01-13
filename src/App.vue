<template>
  <div id="app">
    <header class="app-header">
      <h1>🎨 Генератор цветовых палитр</h1>
      <p>Vue 3 Практика 27-28</p>
    </header>

    <!-- Навигация -->
    <nav class="navigation">
      <button @click="currentDemo = 'reactive'">Пример 1: Реактивность</button>
      <button @click="currentDemo = 'conditional'">Пример 2: Списки</button>
      <button @click="currentDemo = 'events'">Пример 3: События</button>
      <button @click="currentDemo = 'palette'">Генератор палитр</button>
    </nav>

    <!-- Контент -->
    <main class="main-content">
      <ReactiveDemo v-if="currentDemo === 'reactive'" />
      <ConditionalListDemo v-else-if="currentDemo === 'conditional'" />
      <EventComputedDemo v-else-if="currentDemo === 'events'" />
      <PaletteGenerator v-else-if="currentDemo === 'palette'" />
      <div v-else class="welcome">
        <h2>Выберите пример из навигации</h2>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import ReactiveDemo from './components/ReactiveDemo.vue'
import ConditionalListDemo from './components/ConditionalListDemo.vue'
import EventComputedDemo from './components/EventComputedDemo.vue'
import PaletteGenerator from './components/PaletteGenerator.vue'

const currentDemo = ref('palette')
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #f5f5f5;
}

#app {
  min-height: 100vh;
}

.app-header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 2rem;
  text-align: center;
}

.app-header h1 {
  margin-bottom: 0.5rem;
  font-size: 2.5rem;
}

.app-header p {
  opacity: 0.9;
  font-size: 1.1rem;
}

.navigation {
  background-color: white;
  padding: 1rem 2rem;
  display: flex;
  gap: 1rem;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  flex-wrap: wrap;
  justify-content: center;
}

.navigation button {
  padding: 0.5rem 1rem;
  background-color: #667eea;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.navigation button:hover {
  background-color: #5a67d8;
}

.main-content {
  padding: 2rem;
  max-width: 1200px;
  margin: 0 auto;
}

.welcome {
  text-align: center;
  padding: 4rem;
  color: #666;
}
</style>