<template>
  <router-view />
</template>

<script setup>
import { onMounted } from 'vue'
import { useAppStore } from '@/stores/app'

const appStore = useAppStore()

onMounted(() => {
  // Initialize app
  console.log('App mounted')
})
</script>

<style scoped>
/* Global app styles */
</style>