<template>
  <div id="map" ref="map"></div>
</template>

<script>
import config from "../config";
export default {
  name: "Map",
  props: {
    coordinates: {
      type: Object,
      default: () => {
        return { latitude: null, longitude: null };
      }
    }
  },
  data: () => ({
    map: null,
    coords: { lat: null, lng: null },
    marker: null
  }),
  methods: {
    init() {
      const API_KEY = config.map.keys.api_key;
      const script = document.createElement("script");
      script.src = `https://maps.googleapis.com/maps/api/js?key=${API_KEY}&callback=initMap&language=en&region=MX`;
      script.defer = true;

      window.initMap = () => this.initMap();
      document.head.appendChild(script);
    },
    setCurrentCoordinates() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          position => {
            this.coords = {
              lat: position.coords.latitude,
              lng: position.coords.longitude
            };
            this.$emit("update:coordinates", this.coords);
          },
          error => {
            console.log(error.message);
          }
        );
      } else {
        console.log("Your browser does not support geolocation API");
      }
    },
    setMapListeners(marker) {
      google.maps.event.addListener(marker, "dragend", event => {
        this.coords = {
          lat: event.latLng.lat(),
          lng: event.latLng.lng()
        };
        this.$emit("update:coordinates", this.coords);
      });

      google.maps.event.addListener(this.map, "click", event => {
        this.coords = {
          lat: event.latLng.lat(),
          lng: event.latLng.lng()
        };
        this.$emit("update:coordinates", this.coords);
      });
    },
    moveMarker(coords) {
      this.marker.setPosition(coords);
    },
    initMap() {
      if (
        this.coordinates.latitude === null ||
        this.coordinates.longitude === null
      ) {
        this.setCurrentCoordinates();
      }
      {
        this.coords = {
          lat: this.coordinates.latitude,
          lng: this.coordinates.longitude
        };
      }
      /*global google */
      this.map = new google.maps.Map(this.$refs.map, {
        zoom: 4,
        zoomControl: true,
        streetViewControl: false,
        fullscreenControl: false,
        styles: config.map.styles
      });
      this.marker = new google.maps.Marker({
        map: this.map,
        draggable: true
      });
      this.setMapListeners(this.marker);
    }
  },
  mounted() {
    this.init();
  },
  watch: {
    coords(newCoordinates) {
      if (newCoordinates.lat && newCoordinates.lng) {
        this.moveMarker(newCoordinates);
        this.map.panTo(newCoordinates);
      }
    },
    coordinates(newCoordinates) {
      this.coords = {
        lat: newCoordinates.latitude,
        lng: newCoordinates.longitude
      };
    }
  }
};
</script>

<style scoped>
#map {
  width: 100vw;
  height: 100vh;
}
</style>
