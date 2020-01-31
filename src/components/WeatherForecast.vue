<template>
  <b-container>
    <b-row align-h="center">
      <b-col lg="4">
        <b-input-group>
          <b-form-input v-model="text" placeholder="Enter city name"></b-form-input>
          <b-input-group-append>
            <b-button variant="info" @click="search">Search</b-button>
          </b-input-group-append>
        </b-input-group>
      </b-col>
    </b-row>
    
    <b-alert v-model="showDismissibleAlert" variant="danger" dismissible>
      City not found!
    </b-alert>
      
    <weather-card :info="info" @cityDetails="getCityDetails" />
    <city-details :details="cityDetails" />

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
  data () {
    return {
      info: null,
      text: null,
      showDismissibleAlert: false,
      cityDetails: null
    }
  },
  methods: {
    search () {
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
        })
        .catch(() => {
          this.info = null
          this.showDismissibleAlert = true
        })
    },
    getCityDetails (value) {
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
        })
    }
  }
}
</script>

<style>
.pointer{
  cursor: pointer;
}
</style>
