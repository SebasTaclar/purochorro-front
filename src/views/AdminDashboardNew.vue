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
  background: linear-gradient(135deg, var(--secondary) 0%, var(--secondary-mid) 100%);
}
</style>
