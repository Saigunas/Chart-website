<template>
    <div class="card w-75">
      <div class="card-header">
        <span>TV Ratings comparison</span>
      </div>
      <div class="card-body" style="height: 20rem">
        <Line
            v-if="chartReady"
            :options="chartOptions"
            :data="chartData"
        />
      </div>  
    </div>
</template>

<script setup>
import { ref, reactive, onMounted } from 'vue'
import { Line } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, LineElement, CategoryScale, LinearScale, PointElement } from 'chart.js'

let chartReady = ref(false);

const props = defineProps({
  data: Array
})

const netflixTitles = props.data;

let chartData = {
  datasets: [
  { 
    backgroundColor: 'rgba(54, 162, 235, 0.6)',
    borderColor: 'rgba(54, 162, 235, 1)',
    borderWidth: 1,
    data: []
  },
  { 
    backgroundColor: 'rgba(127, 255, 0, 0.6)',
    borderColor: 'rgba(127, 255, 0, 1)',
    borderWidth: 1,
    data: []
  },
  ]
};

const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  scales: {
    y: {
        type: 'linear',
        labels: []
    },
    x: {
      type: 'category',
      labels: []
    },
  },
}

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
);
makeChart(netflixTitles, "TV-PG", "TV-14");

function makeChart(netflixTitles, ratingToCompare1, ratingToCompare2) {
  // Create an array with objects that 
  // have a number of titles with some TV rating

  let ratingCount1 = [];
  let ratingCount2 = [];

  netflixTitles.forEach((row) => {
    let year = row.release_year;
    let rating = row.rating;

    if(rating === ratingToCompare1) {
      const existingIndex = ratingCount1.findIndex((obj) => obj.rating === rating && obj.year === year);
      if (existingIndex >= 0) {
        ratingCount1[existingIndex].count++;
      } else {
        ratingCount1.push({ rating: rating, year: year, count: 1 });
      }
    }

    if(rating === ratingToCompare2) {
      const existingIndex = ratingCount2.findIndex((obj) => obj.rating === rating && obj.year === year);
      if (existingIndex >= 0) {
        ratingCount2[existingIndex].count++;
      } else {
        ratingCount2.push({ rating: rating, year: year, count: 1 });
      }
    }
    
  });

  var ratingYears = netflixTitles.map(function (d) {
    return d.release_year;
  });

  // Sort ratings to have a linear value change
  ratingCount1 = ratingCount1
    .sort(function(a, b) {
      return a.year - b.year;
    });
  ratingCount2 = ratingCount2
  .sort(function(a, b) {
    return a.year - b.year;
  });

  // Set x label to years
  chartOptions.scales.x.labels = ratingYears
    .filter((x, i, a) => {
        return a.indexOf(x) == i
      })
    .sort(function(a, b) {
      return a - b;
    });

  
  // Set the right x and y data for the linear graph
  ratingCount1 = ratingCount1.map((d) => {return {y: d.count, x: d.year}})
  ratingCount2 = ratingCount2.map((d) => {return {y: d.count, x: d.year}})

  chartData.datasets[0].label = ratingToCompare1;
  chartData.datasets[0].data = ratingCount1;

  chartData.datasets[1].label = ratingToCompare2;
  chartData.datasets[1].data = ratingCount2;

  chartReady.value = true;
}

</script>

<style>
</style>