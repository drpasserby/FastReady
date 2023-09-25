<template>
  <div class="mainapp">
    <MainTitle />
    <TakeList />
  </div>
</template>

<script setup>
import MainTitle from './components/MainTitle.vue'
import TakeList from './components/TakeList.vue'
</script>

<style scoped>
.mainapp{
  width: 100%;
  margin: .5em auto;
  padding: .5em;
}
</style>
