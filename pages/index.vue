<template>
  <v-layout
    row wrap
    justify-center
    align-center
  >
    <v-flex xs12 md6 px-2>
      <v-card>
        <v-card-title class="headline text-center">
          Heat Map
        </v-card-title>
        <heatmap></heatmap>
      </v-card>
    </v-flex>
    <v-flex xs12 md6 px-2>
      <v-card>
        <v-card-title class="headline text-center">
          Parallel
        </v-card-title>
        <parallel></parallel>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import Heatmap from '../components/Heatmap.vue';
import Parallel from "../components/Parallel";

export default {
  data: () => ({
  }),
  components: {
    Heatmap,
    Parallel
  }
}
</script>

<style>
body {
  background: #202020
}
</style>
