<script setup>
import Header from './components/Header.vue'
import User from "@/components/User.vue";
import Sidebar from "@/components/Sidebar.vue";
</script>

<template>
  <div id="app">
    <Header title="Authentication Management"/>
    <div class="wrapper">
      <Sidebar title="Sidebar"/>
      <main>
        <User/>
      </main>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  display: grid;
  grid-template-columns: 20% 80%;
  height: 100vh;
  padding: 0 0;
}
</style>
