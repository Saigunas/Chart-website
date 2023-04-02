<script setup>
  import { ref } from "vue";
  import Home from "./components/Home.vue"
  import Chart from "./components/Chart.vue"

  let activeTab = ref('Home');
</script>

<template>
  <div id="app-body">
    <nav class="navbar navbar-dark bg-dark navbar-expand-lg navbar-light bg-light">
        <a class="navbar-brand ps-4" href="#">Chart app</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNavAltMarkup" aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
          <div class="navbar-nav">
            <a @click="activeTab = 'Home'" class="nav-item nav-link active" href="#">Home</a>
            <a @click="activeTab = 'Chart'" class="nav-item nav-link" href="#">Chart</a>
            <a @click="activeTab = 'About'" class="nav-item nav-link" href="#">About</a>
          </div>
        </div>
    </nav>

    <home v-if="activeTab === 'Home'"/>

    <chart v-if="activeTab === 'Chart'"/>

    <div v-if="activeTab === 'About'">
      About
    </div>

    <footer class="footer bg-dark text-white py-2">
        <div class="container">
          <div class="row">
            <div class="col-md-6 d-flex align-items-center gap-2">
              <a href="https://github.com/Saigunas/Chart-website"><h3 class="m-0 bi bi-github"></h3></a>
              <span>University project for data visualization</span>
            </div>
            <div class="col-md">
            </div>
          </div>
        </div>
    </footer>
  </div>
</template>

<style scoped>
#app-body {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  min-height: 100vh;
}
</style>
