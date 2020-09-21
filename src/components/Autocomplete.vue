<template>
  <v-autocomplete
    label="Find location"
    hide-details
    rounded
    solo
    clearable
    v-model="select"
    :items="items"
    :search-input.sync="search"
  ></v-autocomplete>
</template>

<script>
export default {
  name: "Autocomplete",
  data() {
    return {
      items: [],
      search: null,
      select: null,
      informationCountry: null,
      timer: null
    };
  },
  methods: {
    querySelections(country) {
      let url = `https://restcountries.eu/rest/v2/name/${country}`;
      this.items = [];
      fetch(url)
        .then(response => {
          return response.json();
        })
        .then(data => {
          this.items = data.map(element => ({
            text: element.name,
            value: element
          }));

          this.$emit("locationDetail", this.select);
        })
        .catch(error => console.error(error));
    }
  },
  watch: {
    search(value) {
      console.log("Value: ", value);
      clearTimeout(this.timer);
      this.timer = setTimeout(() => {
        if (value !== "" && value !== null) {
          this.querySelections(value);
        }
      }, 1000);
    }
  }
};
</script>

<style scoped></style>
