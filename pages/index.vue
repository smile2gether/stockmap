<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <v-flex xs12>
      <v-card>
        <v-card-title class="headline text-center">
          Welcome to Stock Map!
        </v-card-title>
        <!--<line-chart></line-chart>-->
        <heatmap></heatmap>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import Heatmap from '../components/Heatmap.vue'
// import LineChart from "../components/LineChart";

export default {
  data: () => ({
  }),
  components: {
    // LineChart,
    'heatmap': Heatmap
  }
}
</script>

<style>
body {
  background: #202020
}
</style>
