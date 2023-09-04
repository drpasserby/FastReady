<script setup>
import MainTitle from './components/MainTitle.vue'
import TakeList from './components/TakeList.vue'
</script>

<template>
  <div class="mainapp">
    <MainTitle />
    <TakeList />
  </div>
</template>

<style scoped>
.mainapp{
  width: 100%;
  margin: .5em auto;
  padding: .5em;
}
</style>
