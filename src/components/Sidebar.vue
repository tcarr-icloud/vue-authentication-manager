<script setup>
import SwaggerUI from "@/components/SwaggerUI.vue";

defineProps({
  title: {
    type: String,
    required: true,
  },
})
</script>

<template>
  <div class="sidebar">
    <h1>
      {{ title }}
    </h1>
    <SwaggerUI/>
    <a href="" target="_blank" rel="noopener noreferrer">Users</a>
  </div>
</template>

<style scoped>
.sidebar {
  padding: .5rem;
}
</style>