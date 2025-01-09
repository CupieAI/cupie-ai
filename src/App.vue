<script setup>
import { ref } from 'vue'
import Navbar from './components/Navbar.vue'
import Main from './components/Main.vue'

const currentPage = ref('Main')
</script>

<template>
  <div class="app-container">
    <Navbar @navigate="currentPage = $event" />
    <main class="main-content">
      <div class="content-wrapper">
        <component :is="currentPage === 'Main' ? Main : currentPage === 'Pump.fun' ? Pumpfun : Dexscreener" />
      </div>
    </main>
  </div>
</template>

<style>
.app-container {
  min-height: 100vh;
  background-image: url('./assets/background.jpeg');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

.main-content {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1rem;
}

.content-wrapper {
  width: 80vw;
  height: 75vh;
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(4px);
  -webkit-backdrop-filter: blur(10px);
  border-radius: 1rem;
  padding: 2rem;
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: center;
  text-align: center;
}

h1 {
  margin-bottom: 1.5rem;
  color: #ff6b9d;
  font-size: 2.5rem;
}

h2{
  color: #ff6b9d;
}

p {
  color: #ff6b9d;
  margin-bottom: 1rem;
  font-size: 1.2rem;
}
</style>