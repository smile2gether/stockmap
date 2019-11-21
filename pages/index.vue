<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <v-flex
      xs12
      sm8
      md6
    >
      <v-card>
        <v-card-title class="headline">
          Welcome to Stock Map!
        </v-card-title>
        <heatmap></heatmap>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import Heatmap from '../components/Heatmap.vue'

export default {
  data: () => ({
  }),
  components: {
    'heatmap': Heatmap
  }
}
</script>
