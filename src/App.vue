<template>
  <v-app>
    <v-main>
      <v-btn class="mt-5 ml-5" fab absolute color="pink" dark @click="setCoordinates">
        <v-icon>my_location</v-icon>
      </v-btn>
      <Map ref="map" :coordinates.sync="coordinates" />
    </v-main>
  </v-app>
</template>

<script>
import Map from "./components/Map";

export default {
  name: "App",
  components: {
    Map
  },
  data: () => ({
    coordinates: {
      latitude: null,
      longitude: null
    }
  }),
  methods: {
    setCoordinates() {
      this.$refs.map.setCurrentCoordinates();
    }
  }
};
</script>
