<script setup>
defineProps({
  title: {
    type: String,
    required: true,
  },
})
</script>

<template>
  <div class="header">
    <img alt="Vue logo" class="logo" src="../assets/logo.svg" width="50" height="50"/>
    <h1>{{ title }}</h1>
  </div>
</template>

<style scoped>
.header {
  display: flex;
  align-items: center;
}

.logo {
  margin-right: 1rem;
}

h1 {
  font-weight: 500;
  font-size: 2.6rem;
}
</style>
