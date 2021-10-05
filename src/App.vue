<template>
  <p>Data Loading: {{ dataPromise.isPending }} (should take 2 seconds)</p>
  <p>
    this chart has
    <code>series="[]"</code> before the data is loaded
  </p>
  <div class="a">
    <ApexChart
      type="line"
      :options="{
        chart: {
          height: 100,
        },
        xaxis: {
          type: 'datetime',
        },
        noData: {
          text: dataPromise.isPending ? 'Loading...' : dataPromise.error ? 'Error' : 'No Data'
        },
      }"
      :series="series"
    />
  </div>
  <p>
    This chart has
    <code>series="[ { data: [] } ]"</code>
  </p>
  <div class="b">
    <ApexChart
      type="line"
      :options="{
        chart: {
          height: 100,
        },
        noData: {
          text: 'No data as expected'
        },
      }"
      :series="[{ data: [] }]"
    />
  </div>
  <button
    onclick="alert('clicked button');"
  >This button can't be clicked on most of it because it is covered by the chart.</button>
  <p>If you change some code and hit save, Vite's watcher will referesh the
    page and the chart will resize to the correct height.</p>
</template>

<script lang="ts">
import { defineComponent, reactive, computed } from 'vue'
import ApexChart from 'vue3-apexcharts'
import { usePromise } from 'vue-promised'

async function fetchData() {
  await new Promise(function (resolve) {
    setTimeout(resolve, 2000) //change this number to make data loading longer/shorter
  })
  return [{ data: [['2021-01-01', 3], ['2021-03-09', 5], ['2021-09-10', 7]] }]
}

export default defineComponent({
  components: { ApexChart },
  setup() {
    const dataPromise = reactive(usePromise(fetchData()))
    const series = computed(() => {
      if (!dataPromise.data) {
        return []
      } else {
        return dataPromise.data
      }
    })

    return { dataPromise, series }
  }
})
</script>

<style scoped>
.a {
  width: 300px;
  height: 101px;
  border: 2px solid blue;
  margin-bottom: 250px;
}

.b {
  width: 300px;
  height: 101px;
  border: 2px solid red;
}
</style>