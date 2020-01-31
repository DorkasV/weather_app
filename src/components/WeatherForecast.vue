<template>
  <b-container>
    <b-row align-h="center">
      <b-col lg="4">
        <b-input-group>
          <b-form-input v-model="text" placeholder="Enter city name" @keyup.enter="search"></b-form-input>
          <b-input-group-append>
            <b-button variant="info" @click="search">Search</b-button>
          </b-input-group-append>
        </b-input-group>
      </b-col>
    </b-row>

    <b-row align-h="center">
      <b-col lg="4">
        <b-alert v-model="showDismissibleAlert" variant="danger" dismissible>
          City not found!
        </b-alert>
      </b-col>
    </b-row>

    <weather-card :info="info" @cityDetails="getCityDetails" />
    <city-details :details="cityDetails" />

    <div v-if="showLoader" class="d-flex justify-content-center mb-3">
      <b-spinner label="Loading..."></b-spinner>
    </div>

  </b-container>
</template>

<script>
const axios = require('axios')
import WeatherCard from './WeatherCard.vue'
import CityDetails from './CityDetails.vue'
export default {
  components: {
    WeatherCard,
    CityDetails
  },
  data() {
    return {
      info: null,
      text: null,
      showDismissibleAlert: false,
      cityDetails: null,
      showLoader: false
    }
  },
  methods: {
    search() {
      this.showLoader = true
      this.cityDetails = null
      axios
        .get('https://api.openweathermap.org/data/2.5/weather', {
          params: {
            q: this.text,
            appid: 'b29f837cd55f0cd934760650350ebe84',
            units: 'metric'
          }
        })
        .then(response => {
          this.showDismissibleAlert = false
          if (response) (this.info = response.data)
          this.showLoader = false
        })
        .catch(() => {
          this.info = null
          this.showDismissibleAlert = true
          this.showLoader = false
        })
    },
    getCityDetails(value) {
      this.showLoader = true
      axios
        .get('https://api.openweathermap.org/data/2.5/forecast', {
          params: {
            id: value,
            appid: 'b29f837cd55f0cd934760650350ebe84',
            units: 'metric'
          }
        })
        .then(response => {
          if (response) (this.cityDetails = response.data)
          this.showLoader = false
        })
    }
  }
}
</script>

<style>
.pointer {
  cursor: pointer;
}
</style>

