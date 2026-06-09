<template>
  <div class="admin-page">
    <AdminDashboardSimple />
  </div>
</template>

<script setup lang="ts">
import AdminDashboardSimple from '@/components/AdminDashboardSimple.vue'
</script>

<style scoped>
.admin-page {
  min-height: 100vh;
  background: linear-gradient(135deg, #071e25 0%, #0b2a33 100%);
}
</style>
