<template>
  <header class="app-header">
    <div class="header-container">
      <div class="header-left">
        <button @click="toggleSidebar" class="menu-toggle">
          <span class="menu-icon">☰</span>
        </button>
        <router-link to="/" class="logo">
          <h1>{{ appTitle }}</h1>
        </router-link>
      </div>
      
      <nav class="header-nav">
        <router-link to="/" class="nav-link">Home</router-link>
        <router-link to="/about" class="nav-link">About</router-link>
        <router-link to="/users" class="nav-link">Users</router-link>
      </nav>
      
      <div class="header-right">
        <div v-if="isAuthenticated" class="user-menu">
          <span class="user-name">{{ user?.name || 'User' }}</span>
          <button @click="handleLogout" class="btn-logout">Logout</button>
        </div>
        <router-link v-else to="/login" class="btn-login">Login</router-link>
      </div>
    </div>
  </header>
</template>

<script setup>
import { computed } from 'vue'
import { useRouter } from 'vue-router'
import { useUserStore } from '@/stores/user'
import { useAppStore } from '@/stores/app'

const router = useRouter()
const userStore = useUserStore()
const appStore = useAppStore()

const appTitle = computed(() => import.meta.env.VITE_APP_TITLE || 'Vue Seed Project')
const isAuthenticated = computed(() => userStore.isAuthenticated)
const user = computed(() => userStore.user)

const toggleSidebar = () => {
  appStore.toggleSidebar()
}

const handleLogout = async () => {
  await userStore.logout()
  router.push('/')
}
</script>

<style scoped>
.app-header {
  background: var(--color-primary);
  color: white;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  position: sticky;
  top: 0;
  z-index: 100;
}

.header-container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 var(--spacing-md);
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 64px;
}

.header-left {
  display: flex;
  align-items: center;
  gap: var(--spacing-md);
}

.menu-toggle {
  background: none;
  border: none;
  color: white;
  font-size: 1.5rem;
  cursor: pointer;
  padding: var(--spacing-sm);
  display: flex;
  align-items: center;
}

.menu-toggle:hover {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 4px;
}

.logo {
  text-decoration: none;
  color: white;
}

.logo h1 {
  margin: 0;
  font-size: 1.5rem;
  font-weight: 600;
}

.header-nav {
  display: flex;
  gap: var(--spacing-lg);
}

.nav-link {
  color: white;
  text-decoration: none;
  padding: var(--spacing-sm) var(--spacing-md);
  border-radius: 4px;
  transition: background 0.2s;
}

.nav-link:hover {
  background: rgba(255, 255, 255, 0.1);
}

.nav-link.router-link-active {
  background: rgba(255, 255, 255, 0.2);
  font-weight: 600;
}

.header-right {
  display: flex;
  align-items: center;
}

.user-menu {
  display: flex;
  align-items: center;
  gap: var(--spacing-md);
}

.user-name {
  font-weight: 500;
}

.btn-logout,
.btn-login {
  background: rgba(255, 255, 255, 0.2);
  color: white;
  border: 1px solid rgba(255, 255, 255, 0.3);
  padding: var(--spacing-sm) var(--spacing-md);
  border-radius: 4px;
  cursor: pointer;
  text-decoration: none;
  font-size: 0.875rem;
  transition: all 0.2s;
}

.btn-logout:hover,
.btn-login:hover {
  background: rgba(255, 255, 255, 0.3);
}

@media (max-width: 768px) {
  .header-nav {
    display: none;
  }
}
</style>