<script setup>
    defineProps({
    isOpen: Boolean
    });
    defineEmits(['close']);
</script>

<template>
  <div v-if="isOpen" class="backdrop" @click="$emit('close')"></div>

  <aside :class="['side-menu', { 'open': isOpen }]">
    <div class="menu-header">
      <h2>History</h2>
      <button class="close-btn" @click="$emit('close')">×</button>
    </div>

    <ul class="menu-list">
      <li class="new-chat"><span>+</span> New Inquiry</li>
      <li class="divider"></li>
      <li>Surah Al-Baqarah explanations</li>
      <li>Rights of neighbors</li>
      <li>History of Al-Andalus</li>
    </ul>
  </aside>
</template>

<style scoped>
.side-menu {
  position: fixed; /* Floats on top of everything */
  top: 0;
  left: 0;
  height: 100vh;
  width: 280px;
  background-color: #143620;
  color: #FDFBF7;
  z-index: 1000;
  border-right: 1px solid #C5A059;
  
  /* The Animation Magic */
  transform: translateX(-100%); /* Start hidden (moved left) */
  transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 5px 0 15px rgba(0,0,0,0.2);
}

/* When the 'open' class is added, move it back to 0 */
.side-menu.open {
  transform: translateX(0);
}

.backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(0,0,0,0.5); /* Semi-transparent black */
  z-index: 999;
  backdrop-filter: blur(2px);
}

.menu-header {
  padding: 20px;
  border-bottom: 1px solid rgba(197, 160, 89, 0.3);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.close-btn {
  background: none;
  border: none;
  color: #FDFBF7;
  font-size: 1.5rem;
  cursor: pointer;
}

.menu-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.menu-list li {
  padding: 15px 20px;
  cursor: pointer;
  border-bottom: 1px solid rgba(197, 160, 89, 0.1);
  transition: background 0.2s;
}

.menu-list li:hover {
  background-color: #1A472A;
}

.new-chat {
  color: #C5A059;
  font-weight: bold;
}

.divider {
  height: 1px;
  padding: 0 !important;
  background: #C5A059;
  opacity: 0.3;
}
</style>