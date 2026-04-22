<template>
  <div class="container">
    <h1>NYC Criminal Data</h1>

    <div class="chart-wrapper">
      <BarChart :labels="labels" :values="values" />
    </div>

    <table>
      <thead>
        <tr>
          <th>Arrest Key</th>
          <th>Offense</th>
          <th>Category</th>
          <th>Borough</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in data" :key="item.arrest_key">
          <td>{{ item.arrest_key }}</td>
          <td>{{ item.ofns_desc }}</td>
          <td>{{ item.law_cat_cd }}</td>
          <td>{{ item.arrest_boro }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { onMounted } from 'vue'
import BarChart from '../views/BarChart.vue'

const data = ref([])
const labels = ref([])
const values = ref([])

onMounted(async () => {
  try {
    const response = await fetch('https://data.cityofnewyork.us/resource/uip8-fykc.json')
    const result = await response.json()
    data.value = result

    const counts = {}
    result.forEach((item) => {
      const offense = item.ofns_desc || 'Unknown'
      counts[offense] = (counts[offense] || 0) + 1
    })

    labels.value = Object.keys(counts)
    values.value = Object.values(counts)
  } catch (error) {
    console.log('Error', error)
  }
})
</script>

<style scoped>
.container {
  width: 100%;
  min-height: 100vh;
  padding: 2rem;
}

h1 {
  text-align: center;
  margin-bottom: 2rem;
}

.chart-wrapper {
  width: 100%;
  margin-bottom: 2rem;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th,
td {
  border: 1px solid #ccc;
  padding: 0.5rem;
  text-align: left;
}

th {
  background-color: #f0f0f0;
}
</style>
