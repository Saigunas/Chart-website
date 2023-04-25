<script setup>
  import { ref } from "vue";
  import Chart from "./Chart.vue"

  let activeTab = ref('Home');

</script>

<template>
  <div class="about-container">
    <h2>Welcome!</h2>
    <div class="about-border"></div>
    <p>
      This is a university project for data visualization. We thought that it would be a great idea, instead of using
      a program such as Excel that works with data utilization and representation,
      Instead we would use programming language JavaScript and Vue.JS Framework
    </p>
    <h2>Our Charts</h2>
    
    <div class="about-border"></div>
      <p>
        Head over the <a @click="activeTab = 'Chart'" class="chart-view" href="#">Chart</a>  tab to check our Charts made about Netflix viewership, where we've visualized and analysed almost a decade worth of Information
        about Movies and TV shows on Netflix
        <chart v-if="activeTab === 'Chart'"/>
    </p>
  </div>
  
  </template>

<style>
.about-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 50px 20px;
}

.chart-view {
  text-decoration: underline;
  color: blue;
}


.about-border {
  height: 5px;
  width: 60%;
  background-color: black;
  margin-bottom: 30px;
}

h2 {
  text-align: center;
  margin-bottom: 30px;
  margin-top: 0;
  transition: margin-top 0.5s ease, transform 0.3s ease, border-bottom 0.3s ease;
}

h2:hover {
  margin-top: -15px;
  transition: margin-top 0.5s ease, transform 0.3s ease, border-bottom 0.3s ease;

}

p {
  padding: 20px;
  width: 80%;
  text-align: center;
  line-height: 1.5;
  color: gray
}

.content,
h2 {
  animation: fadeIn 0.5s ease;
}

.section {
  position: relative;
  margin-bottom: 50px;
  animation: slideInUp 0.5s ease;
}

.section .title {
  position: absolute;
  top: -50px;
  left: 50%;
  transform: translateX(-50%);
  background-color: #fff;
  padding: 0 20px;
  font-size: 20px;
  font-weight: bold;
  border: 1px solid #ddd;
  border-radius: 5px;
  animation: slideInDown 0.5s ease;
}

@keyframes slideInDown {
  0% {
    transform: translateY(-100%);
  }
  100% {
    transform: translateY(0);
  }
}

@keyframes slideInUp {
  0% {
    transform: translateY(100%);
  }
  100% {
    transform: translateY(0);
  }
}

@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>