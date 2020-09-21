<template>
  <v-app>
    <v-main>
      <div id="controls-container">
        <Autocomplete
          class="searcher"
          @locationDetail="locationDetail($event)"
        />
        <v-btn
          class="btn-current-location"
          fab
          absolute
          color="pink"
          dark
          @click="setCoordinates"
        >
          <v-icon>my_location</v-icon>
        </v-btn>
      </div>
      <Map ref="map" :coordinates.sync="coordinates" />
    </v-main>
  </v-app>
</template>

<script>
import Map from "./components/Map";
import Autocomplete from "./components/Autocomplete";

export default {
  name: "App",
  components: {
    Autocomplete,
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
    },
    locationDetail(detail) {
      if (detail) {
        this.coordinates = {
          latitude: detail.latlng[0],
          longitude: detail.latlng[1]
        };
      }
    }
  }
};
</script>
<style socped lang="scss">
#controls-container {
  position: absolute;
  top: 0;
  z-index: 1;
  padding-left: 20px;
  height: 100%;
  .btn-current-location {
    bottom: 35px;
  }
  .searcher {
    top: 10px;
    width: 500px !important;
  }
}
</style>
