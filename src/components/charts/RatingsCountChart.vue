<template>
    <div class="card w-75">
      <div class="card-header">
        <span>All time total Title TV ratings</span>
      </div>
      <div class="card-body" style="height: 20rem">
        <bar
            v-if="chartReady"
            :options="chartOptions"
            :data="chartData"
        />
      </div>  
    </div>
</template>

<script setup>
import { ref, reactive, onMounted } from 'vue'
import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale, PointElement } from 'chart.js'

let chartReady = ref(false);

const props = defineProps({
  data: Array
})

const netflixTitles = props.data;

let chartData = {
  datasets: [
  { 
    label: 'Number of titles',
    backgroundColor: 'rgba(54, 162, 235, 0.6)',
    borderColor: 'rgba(54, 162, 235, 1)',
    borderWidth: 1,
    data: []
  },
  ]
};

const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  scales: {
    x: {
      type: 'category',
      labels: []
    },
  },
}

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale, PointElement)
makeChart(netflixTitles)

function makeChart(netflixTitles) {
    // Create an array with objects that 
    // have a number of titles with some TV rating

  const ratingCount = [];

  netflixTitles.forEach((row) => {
    const rating = row.rating;

    const existingIndex = ratingCount.findIndex((obj) => obj.rating === rating);
    if (existingIndex >= 0) {
      ratingCount[existingIndex].count++;
    } else {
      ratingCount.push({ rating: rating, count: 1 });
    }
  });

  // Set labels to unique values
  chartOptions.scales.x.labels = ratingCount.map((d) => d.rating);
  
  chartData.datasets[0].data = ratingCount.map((d) => d.count);
  console.log(ratingCount);
  chartReady.value = true;
}

</script>

<style>
</style>