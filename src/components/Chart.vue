<template>
  <div class='w-100 d-flex flex-column align-items-center gap-4 my-4'>
    <ratings-count-chart
      v-if="dataSetReady"
      :data="chartData"
    />
    <ratings-by-year-chart
      v-if="dataSetReady"
      :data="chartData"
    />
  </div>  
</template>

<script setup>
import { ref, reactive, onMounted } from 'vue'
import * as d3 from 'd3';
import RatingsByYearChart from './charts/RatingsByYearChart.vue';
import RatingsCountChart from  './charts/RatingsCountChart.vue';

const dataSetReady = ref(false);
const chartData = ref([]);

d3.csv("src/assets/netflix_titles2.csv").then(prepareData);

function prepareData(data) {
  chartData.value = data;
  dataSetReady.value = true;
}

</script>

<style>
</style>