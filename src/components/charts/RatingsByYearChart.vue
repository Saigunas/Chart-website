<template>
    <div class="card w-75">
        <div class="card-header">
            <span>Title TV ratings by year (2000>)</span>
        </div>
        <div class="card-body" style="height: 35rem">
            <Bubble
                v-if="chartReady"
                :options="chartOptions"
                :data="chartData"
            />
        </div>
    </div>  
</template>

<script setup>
import { ref, reactive, onMounted } from 'vue'
import { Bar, Bubble } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale, PointElement } from 'chart.js'

let chartReady = ref(false);

const props = defineProps({
  data: Array
})

const netflixTitles = props.data;

let chartData = {
  datasets: [
  { 
    label: 'Number of titles (x10)',
    backgroundColor: 'rgba(219, 26, 0, 0.6)',
    borderColor: 'rgba(143, 17, 0, 1)',
    borderWidth: 1,
    data: []
  },
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
    y: {
        type: 'category',
        labels: []
    },
    x: {
      type: 'category',
      labels: []
    },
  },
  plugins: {
    legend: {
        position: 'bottom',
        display: true,
            labels: {
                padding: 20
            }
        }
    }
}

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale, PointElement)
makeChart(netflixTitles)

function makeChart(netflixTitles) {
  var titlesRating = netflixTitles.map(function (d) {
    return d.rating;
  });
  var ratingYears = netflixTitles.map(function (d) {
    return d.release_year;
  });

  const ratingsByYear = [];

  netflixTitles.forEach((row) => {
    const x = row.release_year;
    const y = row.rating;

    if (x >= 2000) {
      const existingIndex = ratingsByYear.findIndex((obj) => obj.x === x && obj.y === y);
      if (existingIndex >= 0) {
        ratingsByYear[existingIndex].r++;
      } else {
        ratingsByYear.push({ x, y, r: 1 });
      }
    }
  });
  
  const ratingsByYearUnder50 = ratingsByYear.filter(d => d.r < 50);
  const ratingsByYearOver50 = ratingsByYear
    .filter(d => d.r > 50)
    .map(d => {d.r = d.r / 10; return d;});

  chartData.datasets[0].data = ratingsByYearOver50;
  chartData.datasets[1].data = ratingsByYearUnder50;
  
  // Set labels to unique values
  chartOptions.scales.y.labels = titlesRating.filter((x, i, a) => a.indexOf(x) == i);
  chartOptions.scales.x.labels = ratingYears
    .filter((x, i, a) => {
        return a.indexOf(x) == i && x >= 2000
      })
    .sort(function(a, b) {
      return a - b;
    });
  console.log(ratingsByYearOver50);
  chartReady.value = true;
}

</script>

<style>
</style>