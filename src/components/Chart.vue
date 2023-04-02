<template>
  <Bubble
    v-if="chartReady"
    id="my-chart-id"
    :options="chartOptions"
    :data="chartData"
  />
</template>

<script setup>
import { ref, reactive, onMounted } from 'vue'
import { Bar, Bubble } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale, PointElement } from 'chart.js'
import * as d3 from 'd3';

d3.csv("src/assets/netflix_titles2.csv").then(makeChart);

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale, PointElement)

function makeChart(netflixTitles) {
  var titlesRating = netflixTitles.map(function (d) {
    return d.rating;
  });
  var titlesReleaseYear = netflixTitles.map(function (d) {
    return d.release_year;
  });

  const yearsTvRatings = {};
  const titleCountUnder50 = [];
  const titleCountOver50 = [];

  netflixTitles.forEach((row) => {
    const year = row.release_year;
    const rating = row.rating;

    if(year < 2000) {return;}
    if (!yearsTvRatings[year]) {
      yearsTvRatings[year] = {};
    }
    if (!yearsTvRatings[year][rating]) {
      yearsTvRatings[year][rating] = 0;
    }
    yearsTvRatings[year][rating]++;
  });

  Object.entries(yearsTvRatings).forEach(yearObj => {
    const [year, ratingObjs] = yearObj;
    Object.entries(ratingObjs).forEach(ratingObj => {
      const [rating, count] = ratingObj;

      const ratingYearCount = {};
      ratingYearCount.x = year;
      ratingYearCount.y = rating;
      ratingYearCount.r = count;

      if(ratingYearCount.r > 50) {
        ratingYearCount.r = ratingYearCount.r / 10;
        titleCountOver50.push(ratingYearCount);
      } else {
        titleCountUnder50.push(ratingYearCount);
      }
    })
  });


  chartData.datasets[0].data = titleCountOver50;
  chartData.datasets[1].data = titleCountUnder50;
  // Set labels to unique values
  chartOptions.scales.y.labels = titlesRating.filter((x, i, a) => a.indexOf(x) == i);
  
  chartReady.value = true;
}
let chartData = reactive({
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
});

const chartOptions = {
  responsive: true,
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
}
let chartReady = ref(false);

</script>

<style>
</style>