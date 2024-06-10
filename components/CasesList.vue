<script setup lang="ts">
const casesData = ref([])
const sourcesList = ref([])

casesData.value = (await queryContent('cases').findOne()).cases;
sourcesList.value = Array.from(new Set(casesData.value.map((c) => c.source)))
</script>

<template>
  <h2>Sources</h2>
  <ul>
    <li v-for="source in sourcesList" :key="source">
      <a :href="source" target="_blank">{{ source }}</a>
    </li>
  </ul>
</template>

<style scoped>

</style>