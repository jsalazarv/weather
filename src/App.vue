<template>
  <v-app>
    <Layout>
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
      <Map class="map" ref="map" :coordinates.sync="coordinates" />
    </Layout>
  </v-app>
</template>

<script>
import Map from "./components/Map";
import Autocomplete from "./components/Autocomplete";
import Layout from "./components/Layout";

export default {
  name: "App",
  components: {
    Layout,
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
.map {
  position: absolute;
  height: 100%;
  width: 100%;
}
#controls-container {
  position: absolute;
  top: 0;
  z-index: 1;
  padding-left: 20px;
  height: 100%;
  .searcher {
    top: 10px;
    width: 500px !important;
  }
  .btn-current-location {
    bottom: 35px;
  }
}
</style>
