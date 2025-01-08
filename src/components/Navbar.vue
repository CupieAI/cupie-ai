<script setup>
const menuItems = [
  { name: 'Main', path: 'Main', internal: true },
  { name: 'Pump.fun', path: '#pumpfun', external: true },
  { name: 'Dexscreener', path: '#dexscreener', external: true }
]

const emit = defineEmits(['navigate'])

const handleClick = (item) => {
  if (item.internal) {
    emit('navigate', item.name)
  } else {
    window.open(item.path, '_blank')
  }
}
</script>

<template>
  <nav class="navbar">
    <div class="nav-content">
      <ul class="nav-menu">
        <li v-for="item in menuItems" :key="item.name">
          <a 
            :href="item.internal ? '#' : item.path"
            @click.prevent="handleClick(item)"
            :target="item.external ? '_blank' : ''"
            :rel="item.external ? 'noopener noreferrer' : ''"
          >
            {{ item.name }}
          </a>
        </li>
      </ul>
    </div>
  </nav>
</template>

<style scoped>
.navbar {
  background: linear-gradient(45deg, #ff8ebd, #ff6b9d);
  backdrop-filter: blur(5px);
  padding: 1rem;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
}

.nav-content {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  justify-content: center;
  align-items: center;
}

.nav-menu {
  list-style: none;
  display: flex;
  gap: 16rem;
  margin: 0;
  padding: 0;
}

.nav-menu a {
  color: white;
  text-decoration: none;
  font-size: 1.1rem;
  transition: color 0.3s ease;
}

.nav-menu a:hover {
  color: #df37d6;
}

@media (max-width: 768px) {
  .nav-menu {
    gap: 1rem;
  }
  
  .nav-menu a {
    font-size: 1rem;
  }
}
</style>