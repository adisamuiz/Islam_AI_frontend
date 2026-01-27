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
      <h2>Resources</h2>
      <button class="close-btn mobile-only" @click="$emit('close')">×</button>
    </div>

    <div class="menu-content">
      
      <div class="section-title">THE NOBLE QURAN</div>
      <ul class="resource-list">
        <li>
          <a href="https://quran.com" target="_blank" class="resource-btn">
            <span class="icon">📖</span>
            <div class="text">
              <span class="main">Read Quran</span>
              <span class="sub">Quran.com</span>
            </div>
            <span class="arrow">↗</span>
          </a>
        </li>
        <li>
          <a href="https://quran.com" target="_blank" class="resource-btn">
            <span class="icon">📜</span>
            <div class="text">
              <span class="main">Tafsir (Exegesis)</span>
              <span class="sub">Ibn Kathir & Others</span>
            </div>
            <span class="arrow">↗</span>
          </a>
        </li>
      </ul>

      <div class="section-title">SUNNAH & HADITH</div>
      <ul class="resource-list">
        <li>
          <a href="https://sunnah.com/bukhari" target="_blank" class="resource-btn">
            <span class="icon">📚</span>
            <div class="text">
              <span class="main">Sahih Al-Bukhari</span>
              <span class="sub">Authentic Hadith</span>
            </div>
            <span class="arrow">↗</span>
          </a>
        </li>
        <li>
          <a href="https://sunnah.com/muslim" target="_blank" class="resource-btn">
            <span class="icon">📚</span>
            <div class="text">
              <span class="main">Sahih Muslim</span>
              <span class="sub">Authentic Hadith</span>
            </div>
            <span class="arrow">↗</span>
          </a>
        </li>
        <li>
          <a href="https://sunnah.com" target="_blank" class="resource-btn">
            <span class="icon">🏛️</span>
            <div class="text">
              <span class="main">All Collections</span>
              <span class="sub">Sunnah.com</span>
            </div>
            <span class="arrow">↗</span>
          </a>
        </li>
      </ul>
      <div class="menu-footer">
          <small>External links provided by Quran.com & Sunnah.com</small>
      </div>
    </div>
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
.menu-header h2 {
  margin: 0;
  font-family: 'Cinzel', serif;
  color: #C5A059;
  font-size: 1.2rem;
}

.close-btn {
  background: none;
  border: none;
  color: #FDFBF7;
  font-size: 1.5rem;
  cursor: pointer;
}

.menu-content {
  flex: 1;
  overflow-y: auto;
  padding: 10px 0;
}

/* SECTION STYLES */
.section-title {
  padding: 20px 20px 10px;
  font-size: 0.75rem;
  letter-spacing: 2px;
  color: rgba(253, 251, 247, 0.5);
  font-family: 'Lato', sans-serif;
  font-weight: bold;
}

.resource-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

/* BUTTON STYLES */
.resource-btn {
  display: flex;
  align-items: center;
  padding: 15px 20px;
  text-decoration: none; /* Remove blue underline */
  color: #FDFBF7;
  border-bottom: 1px solid rgba(197, 160, 89, 0.1);
  transition: background-color 0.2s, padding-left 0.2s;
}

.resource-btn:hover {
  background-color: #1A472A;
  padding-left: 25px; /* Slight slide effect on hover */
  border-left: 4px solid #C5A059;
}

.icon {
  font-size: 1.4rem;
  margin-right: 15px;
  opacity: 0.9;
}

.text {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.main {
  font-weight: bold;
  font-size: 0.95rem;
}

.sub {
  font-size: 0.75rem;
  color: rgba(253, 251, 247, 0.6);
  margin-top: 2px;
}

.arrow {
  color: #C5A059;
  font-size: 0.8rem;
  opacity: 0.5;
}

.menu-footer {
  padding: 15px;
  text-align: center;
  border-top: 1px solid rgba(197, 160, 89, 0.2);
  font-size: 0.7rem;
  color: rgba(253, 251, 247, 0.4);
}

/* Mobile Tweaks */
.mobile-only { display: none; }

@media (max-width: 768px) {
  .side-menu {
    position: absolute;
    height: 100%;
    margin-left: 0;
    transform: translateX(-100%);
    width: 85%; /* Slightly narrower on mobile */
  }
  .side-menu.open {
    transform: translateX(0);
  }
  .mobile-only {
    display: block;
    background: none;
    border: none;
    color: white;
    font-size: 1.5rem;
  }
}</style>