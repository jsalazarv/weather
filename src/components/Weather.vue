<template>
  <div>
    <v-list>
      <v-list-item>
        <v-list-item-content>
          <v-card-text class="pa-10">
            <v-row align="center">
              <v-col class="display-1" cols="6">
                {{ celsiusDegrees }}&deg;C
              </v-col>
              <v-col cols="6">
                <v-img
                  :src="imageUrl"
                  :alt="detail.weather[0].description"
                ></v-img>
              </v-col>
            </v-row>
          </v-card-text>
        </v-list-item-content>
      </v-list-item>
    </v-list>
    <v-divider></v-divider>
    <v-list dense>
      <v-list-item>
        <v-list-item-icon>
          <v-icon>mdi-temperature-celsius</v-icon>
        </v-list-item-icon>
        <v-list-item-content>
          <v-list-item-title>{{ celsiusDegrees }}&deg;C</v-list-item-title>
        </v-list-item-content>
      </v-list-item>

      <v-list-item>
        <v-list-item-icon>
          <v-icon>mdi-clock</v-icon>
        </v-list-item-icon>
        <v-list-item-content>
          <v-list-item-title>{{ detail.timezone }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>

      <v-list-item>
        <v-list-item-icon>
          <v-icon>mdi-weather-windy</v-icon>
        </v-list-item-icon>
        <v-list-item-content>
          <v-list-item-title>{{ detail.wind.speed }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>

      <v-list-item>
        <v-list-item-icon>
          <v-icon>mdi-weather-cloudy</v-icon>
        </v-list-item-icon>
        <v-list-item-content>
          <v-list-item-title>{{ detail.clouds.all }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>

      <v-list-item>
        <v-list-item-icon>
          <v-icon>mdi-water</v-icon>
        </v-list-item-icon>
        <v-list-item-content>
          <v-list-item-title>{{ detail.main.humidity }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>

      <v-list-item>
        <v-list-item-icon>
          <v-icon>mdi-clock-fast</v-icon>
        </v-list-item-icon>
        <v-list-item-content>
          <v-list-item-title>{{ detail.main.pressure }}</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
    </v-list>
  </div>
</template>

<script>
import config from "../config";
export default {
  name: "Weather",
  data() {
    return {
      detail: {},
      celsiusDegrees: null,
      imageUrl: null
    };
  },
  methods: {
    getWeatherData() {
      const API_KEY = config.openweathermap.keys.api_key;
      let url = `http://api.openweathermap.org/data/2.5/weather?lat=19.12345&lon=-99.123456&appid=${API_KEY}`;

      fetch(url)
        .then(response => {
          return response.json();
        })
        .then(data => {
          this.detail = data;
          this.celsiusDegrees = this.kelvinToCelsius(data.main.temp);
          this.imageUrl = this.imageUrlConstructor(data.weather[0]);

          this.$emit("weatherDetail", {
            imageUrl: this.imageUrl,
            locationName: this.detail.name
          });
        });
    },
    kelvinToCelsius(kelvinDegrees) {
      const absoluteZero = 273.15;
      let celsiusDegrees = kelvinDegrees - absoluteZero;

      return celsiusDegrees.toFixed();
    },
    imageUrlConstructor(detail) {
      let icon = detail.icon;
      return `http://openweathermap.org/img/wn/${icon}@2x.png`;
    }
  },
  mounted() {
    this.getWeatherData();
  }
};
</script>

<style scoped></style>
