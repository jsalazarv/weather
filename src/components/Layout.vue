<template>
  <div id="app">
    <v-app id="inspire">
      <v-app id="inspire">
        <v-navigation-drawer
          v-model="drawer"
          :mini-variant.sync="mini"
          app
          right
        >
          <v-list-item class="px-2">
            <v-list-item-avatar>
              <v-img :src="locationData.imageUrl"></v-img>
            </v-list-item-avatar>
            <v-list-item-title>
              {{ locationData.locationName }}
            </v-list-item-title>
            <v-btn icon @click.stop="mini = !mini">
              <v-icon>mdi-chevron-left</v-icon>
            </v-btn>
          </v-list-item>

          <v-divider></v-divider>

          <Weather @weatherDetail="weatherDetail($event)"></Weather>
        </v-navigation-drawer>

        <v-main>
          <slot></slot>
        </v-main>
      </v-app>
    </v-app>
  </div>
</template>

<script>
import Weather from "./Weather";
export default {
  name: "Layout",
  components: { Weather },
  data() {
    return {
      drawer: true,
      mini: false,
      locationData: {}
    };
  },
  methods: {
    weatherDetail(detail) {
      this.locationData = detail;
    }
  }
};
</script>

<style scoped></style>
