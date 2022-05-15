<script setup lang="ts">
// import { RouterLink, RouterView } from 'vue-router'
// import HelloWorld from '@/components/HelloWorld.vue'
</script>

<template>
  <header>
    <h1>Vordăl</h1>
    <!-- <div>
      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav>
    </div> -->
  </header>

  <RouterView />
</template>

<style>
@import '@/assets/base.css';

#app {
  display: flex;
  height: 100vh;
  flex-direction: column;
  max-width: 595px;
  margin: 0 auto;
  padding-left: 4px;
  padding-right: 4px;
  padding-bottom: 4px;
  font-weight: normal;
}
h1 {
  font-size: 2rem;
  text-align: center;
}
</style>
