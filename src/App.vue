<template>
  <div :class="store.theme === 'light' ? 'light-theme' : 'dark-theme'" class="app-shell">
    <!-- Navbar -->
    <header class="app-header">
      <nav class="nav-container">
        <!-- Logo -->
        <RouterLink to="/" class="brand">
          <span class="brand-icon">✓</span>
          <span class="brand-name">TaskFlow<span class="brand-dot">.</span></span>
        </RouterLink>

        <!-- Navigasyon -->
        <div class="nav-links">
          <RouterLink
            to="/"
            class="nav-link"
            :class="{ 'nav-link-active': route.name === 'todo' }"
          >
            <span class="nav-icon">▤</span>
            <span>Görevler</span>
          </RouterLink>

          <RouterLink
            to="/stats"
            class="nav-link"
            :class="{ 'nav-link-active': route.name === 'stats' }"
          >
            <span class="nav-icon">▥</span>
            <span>İstatistikler</span>
          </RouterLink>

          <RouterLink
            to="/about"
            class="nav-link"
            :class="{ 'nav-link-active': route.name === 'about' }"
          >
            <span class="nav-icon">ⓘ</span>
            <span>Hakkında</span>
          </RouterLink>
        </div>

        <!-- Tema ve durum -->
        <div class="nav-actions">
          <span class="theme-label">
            {{ store.theme === 'dark' ? 'Dark mode' : 'Light mode' }}
          </span>

          <button
            @click="store.toggleTheme()"
            class="theme-toggle"
            :aria-label="store.theme === 'dark' ? 'Açık temaya geç' : 'Koyu temaya geç'"
            :title="store.theme === 'dark' ? 'Açık temaya geç' : 'Koyu temaya geç'"
          >
            <span>{{ store.theme === 'dark' ? '☀' : '☾' }}</span>
          </button>
        </div>
      </nav>
    </header>

    <!-- Sayfa içeriği -->
    <main class="app-main">
      <RouterView />
    </main>

    <!-- Alt bilgi -->
    <footer class="app-footer">
      <p>TaskFlow <span>·</span> Gününü planla, hedeflerine ulaş.</p>
    </footer>
  </div>
</template>

<script setup>
import { useRoute } from 'vue-router'
import { useTodoStore } from './stores/todoStore.js'

const store = useTodoStore()
const route = useRoute()
</script>

<style>
/* Genel uygulama */
.app-shell {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  transition:
    background 0.25s ease,
    color 0.25s ease;
}

.dark-theme {
  background: transparent;
  color: #f4f4f5;
}

.light-theme {
  background: linear-gradient(135deg, #f8f7ff, #f1efff, #fafaff);
  color: #181526;
}

/* Navbar */
.app-header {
  position: sticky;
  top: 0;
  z-index: 50;
  width: 100%;
  border-bottom: 1px solid rgba(255, 255, 255, 0.07);
  background: rgba(11, 13, 18, 0.82);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
}

.nav-container {
  width: min(100% - 40px, 1200px);
  min-height: 76px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 24px;
}

/* Logo */
.brand {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  color: #f4f4f5;
  text-decoration: none;
  flex-shrink: 0;
}

.brand-icon {
  display: grid;
  place-items: center;
  width: 36px;
  height: 36px;
  border-radius: 12px;
  background: linear-gradient(135deg, #a78bfa, #7c3aed);
  color: white;
  font-size: 22px;
  font-weight: 800;
  box-shadow: 0 4px 18px rgba(139, 92, 246, 0.25);
}

.brand-name {
  font-size: 20px;
  font-weight: 750;
  letter-spacing: -0.8px;
}

.brand-dot {
  color: #a78bfa;
}

/* Menü */
.nav-links {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 5px;
  border: 1px solid rgba(255, 255, 255, 0.06);
  border-radius: 14px;
  background: rgba(255, 255, 255, 0.025);
}

.nav-link {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 10px 15px;
  border: 1px solid transparent;
  border-radius: 10px;
  color: #9293a2;
  text-decoration: none;
  font-size: 13px;
  font-weight: 550;
  white-space: nowrap;
  transition: all 0.2s ease;
}

.nav-link:hover {
  color: #f4f4f5;
  background: rgba(255, 255, 255, 0.05);
}

.nav-link-active {
  color: #c4b5fd;
  border-color: rgba(167, 139, 250, 0.14);
  background: rgba(139, 92, 246, 0.13);
}

.nav-link-active:hover {
  color: #ddd6fe;
  background: rgba(139, 92, 246, 0.18);
}

.nav-icon {
  font-size: 16px;
}

/* Tema butonu */
.nav-actions {
  display: flex;
  align-items: center;
  gap: 12px;
  flex-shrink: 0;
}

.theme-label {
  color: #9293a2;
  font-size: 12px;
}

.theme-toggle {
  display: grid;
  place-items: center;
  width: 38px;
  height: 38px;
  border: 1px solid rgba(167, 139, 250, 0.2);
  border-radius: 12px;
  background: rgba(139, 92, 246, 0.1);
  color: #c4b5fd;
  font-size: 20px;
  transition: all 0.2s ease;
}

.theme-toggle:hover {
  transform: translateY(-1px);
  border-color: rgba(167, 139, 250, 0.5);
  background: rgba(139, 92, 246, 0.2);
}

/* Sayfa içeriği */
.app-main {
  flex: 1;
  width: 100%;
}

/* Alt bilgi */
.app-footer {
  padding: 24px 16px;
  text-align: center;
  border-top: 1px solid rgba(255, 255, 255, 0.05);
}

.app-footer p {
  margin: 0;
  color: #71717a;
  font-size: 12px;
}

.app-footer span {
  padding: 0 5px;
  color: #a78bfa;
}

/* Açık tema */
.light-theme .app-header {
  border-bottom-color: rgba(30, 27, 75, 0.1);
  background: rgba(255, 255, 255, 0.82);
}

.light-theme .brand {
  color: #211b36;
}

.light-theme .nav-links {
  border-color: rgba(30, 27, 75, 0.07);
  background: rgba(30, 27, 75, 0.025);
}

.light-theme .nav-link {
  color: #77748a;
}

.light-theme .nav-link:hover {
  color: #30264c;
  background: rgba(139, 92, 246, 0.07);
}

.light-theme .nav-link-active {
  color: #6d28d9;
  border-color: rgba(139, 92, 246, 0.15);
  background: rgba(139, 92, 246, 0.1);
}

.light-theme .theme-label {
  color: #77748a;
}

.light-theme .theme-toggle {
  color: #6d28d9;
  border-color: rgba(139, 92, 246, 0.2);
  background: rgba(139, 92, 246, 0.08);
}

.light-theme .app-footer {
  border-top-color: rgba(30, 27, 75, 0.08);
}

/* Tablet ve telefon */
@media (max-width: 700px) {
  .nav-container {
    width: calc(100% - 24px);
    min-height: 68px;
    gap: 10px;
  }

  .brand {
    gap: 7px;
  }

  .brand-icon {
    width: 32px;
    height: 32px;
    border-radius: 10px;
  }

  .brand-name {
    font-size: 17px;
  }

  .nav-links {
    gap: 2px;
  }

  .nav-link {
    padding: 9px 10px;
  }

  .nav-link span:last-child {
    display: none;
  }

  .nav-icon {
    font-size: 18px;
  }

  .theme-label {
    display: none;
  }

  .nav-actions {
    gap: 0;
  }
}

@media (max-width: 380px) {
  .nav-container {
    width: calc(100% - 16px);
    gap: 6px;
  }

  .brand-name {
    font-size: 15px;
  }

  .nav-link {
    padding: 8px;
  }

  .theme-toggle {
    width: 34px;
    height: 34px;
  }
}
</style>