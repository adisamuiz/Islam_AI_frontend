<script setup>
    import { ref, onMounted } from 'vue';

    defineEmits(['toggle-menu']);

    const hijriDate = ref({ day: '', month: '', year: '' });
    const gregorianDate = ref('');

    // Logic to get the current Hijri date using browser native API
    const getDates = () => {
    const now = new Date();

    // 1. Get Gregorian Date (e.g., "Oct 24, 2023")
    gregorianDate.value = new Intl.DateTimeFormat('en-US', {
        day: 'numeric', month: 'short', year: 'numeric'
    }).format(now);

    // 2. Get Hijri Date
    // 'en-u-ca-islamic-umalqura' asks the browser for the Islamic Calendar
    const hijriParts = new Intl.DateTimeFormat('en-US-u-ca-islamic-umalqura', {
        day: 'numeric',
        month: 'long',
        year: 'numeric'
    }).formatToParts(now);

    // Extract parts into our reactive object
    hijriDate.value = {
        day: hijriParts.find(p => p.type === 'day')?.value || '',
        month: hijriParts.find(p => p.type === 'month')?.value || '',
        year: hijriParts.find(p => p.type === 'year')?.value + ' AH' || ''
    };
    };

    onMounted(() => {
    getDates();
    });
</script>

<template>
  <header class="header">
    
    <div class="header-left">
      <button class="menu-btn" @click="$emit('toggle-menu')" title="Open Menu">
        ☰
      </button>
    </div>

    <div class="header-center">
      <div class="logo-svg">
        <svg viewBox="0 0 100 100" fill="none" xmlns="http://www.w3.org/2000/svg">
          <rect x="20" y="20" width="60" height="60" transform="rotate(45 50 50)" stroke="#C5A059" stroke-width="2"/>
          <rect x="20" y="20" width="60" height="60" stroke="#C5A059" stroke-width="2"/>
          <circle cx="50" cy="50" r="10" fill="#C5A059" fill-opacity="0.2"/>
          <circle cx="50" cy="50" r="4" fill="#C5A059"/>
        </svg>
      </div>
      
      <h1>AL-BAYAN</h1>
      <div class="subtitle">The Clarification</div>
    </div>

    <div class="header-right">
      <div class="hijri-date">
        <span class="day">{{ hijriDate.day }}</span>
        <span class="month">{{ hijriDate.month }}</span>
        <span class="year">{{ hijriDate.year }}</span>
      </div>
      <div class="gregorian-date">{{ gregorianDate }}</div>
    </div>

  </header>
</template>

<style scoped>
.header {
  background-color: #1A472A; /* Deep Green */
  background-image: linear-gradient(to bottom, #1A472A, #143620);
  color: #FDFBF7;
  padding: 10px 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-bottom: 3px double #C5A059; /* Double border looks more "book-like" */
  height: 80px; /* Taller to fit the logo */
  box-shadow: 0 4px 10px rgba(0,0,0,0.15);
  position: relative;
  z-index: 50;
}

/* --- LEFT SECTION --- */
.header-left {
  flex: 1; /* Takes up left space */
  display: flex;
  justify-content: flex-start;
}

.menu-btn {
  background: transparent;
  border: 1px solid rgba(197, 160, 89, 0.3);
  border-radius: 4px;
  color: #C5A059;
  font-size: 1.5rem;
  cursor: pointer;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
}

.menu-btn:hover {
  border-color: #C5A059;
  background-color: rgba(197, 160, 89, 0.1);
  color: #fff;
}

/* --- CENTER SECTION --- */
.header-center {
  flex: 2;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: relative;
  top: 5px; /* Slight visual tweak */
}

.logo-svg {
  width: 40px;
  height: 40px;
  margin-bottom: -5px;
}

.logo-svg svg {
  width: 100%;
  height: 100%;
  filter: drop-shadow(0 0 5px rgba(197, 160, 89, 0.5)); /* Glow effect */
}

h1 {
  font-family: 'Cinzel', serif; /* A very elegant, roman-style font */
  margin: 0;
  font-size: 1.4rem;
  font-weight: 700;
  letter-spacing: 3px;
  color: #FDFBF7;
  text-shadow: 1px 1px 2px rgba(0,0,0,0.3);
}

.subtitle {
  font-family: 'Lato', sans-serif;
  font-size: 0.75rem;
  color: #C5A059;
  text-transform: uppercase;
  letter-spacing: 2px;
  margin-top: 2px;
}

/* --- RIGHT SECTION --- */
.header-right {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  justify-content: center;
  border-left: 1px solid rgba(197, 160, 89, 0.3);
  padding-left: 15px;
  height: 50px;
}

.hijri-date {
  font-family: 'Amiri', serif; /* Arabic-style serif font */
  color: #C5A059;
  font-size: 1.1rem;
  font-weight: bold;
  line-height: 1.2;
}

.gregorian-date {
  font-family: 'Lato', sans-serif;
  font-size: 0.7rem;
  color: rgba(253, 251, 247, 0.6);
  margin-top: 2px;
}

/* Mobile Tweaks */
@media (max-width: 600px) {
  .header {
    padding: 10px;
    height: 70px;
  }
  
  h1 { font-size: 1.1rem; }
  .logo-svg { width: 30px; height: 30px; }
  .subtitle { display: none; } /* Hide subtitle on small screens to save space */
  
  .header-right {
    padding-left: 10px;
  }
  .hijri-date { font-size: 0.9rem; }
  .gregorian-date { font-size: 0.6rem; }
}
</style>