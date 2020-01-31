<template>
  <b-container>
    <h4>Weather Forecast</h4>
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

    <weather-card :info="info" @cityDetails="getCityDetails" @showChart="getTemperatureChart" />
    <city-details v-if="showTemperature" :details="cityDetails" />

    <div v-if="showLoader" class="d-flex justify-content-center mb-3">
      <b-spinner label="Loading..."></b-spinner>
    </div>

    <line-chart v-if="showChart" :details="cityDetails" />

  </b-container>
</template>

<script>
const axios = require('axios')
import WeatherCard from './WeatherCard.vue'
import CityDetails from './CityDetails.vue'
import LineChart from './Chart.vue'
export default {
  components: {
    WeatherCard,
    CityDetails,
    LineChart
  },
  data() {
    return {
      info: null,
      text: null,
      showDismissibleAlert: false,
      cityDetails: null,
      showLoader: false,
      showChart: false,
      showTemperature: false
    }
  },
  methods: {
    showHideTemperatureOrChart () {
      this.showChart = true
      this.showTemperature = false
    },
    search() {
      this.showLoader = true
      this.cityDetails = null
      this.showChart = false
      this.showTemperature = false
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
    getTemperatureChart (value) {
      this.cityDetails = null
      this.showChart = true
      this.showTemperature = false
      this.getDetails(value)
    },
    getCityDetails(value) {
      this.showChart = false
      this.showTemperature = true
      this.getDetails(value)
    },
    getDetails (value) {
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

