<script setup>
import MainTitle from './components/MainTitle.vue'
import SwitchList from './components/SwitchList.vue'
import TakeList from './components/TakeList.vue'
</script>

<template>
  <MainTitle />
  <SwitchList />
  <TakeList />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
