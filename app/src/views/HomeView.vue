<template>
  <div>
    <h1>NYC Criminal Data</h1>

    <div v-for="item in data" :key="item.id">
      <pre>{{ item }}</pre>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { onMounted } from 'vue'

const data = ref([])

onMounted(async () => {
  try {
    const response = await fetch('https://data.cityofnewyork.us/resource/uip8-fykc.json')
    const result = await response.json()
    data.value = result
  } catch (error) {
    console.log('Error')
  }
})
</script>

<style scoped>
h1 {
  text-align: center;
}
</style>
