<template>
  <b-row v-if="info" align-h="center">
    <b-col lg="6">
      <b-card
        tag="article"
        class="mb-2"
      >
        <b-card-title>
          <img alt="Flag" :src="getWeatherIconUrl(info.weather[0].icon)">
          <b-badge variant="light" id="city" class="pointer" @click="$emit('cityDetails', info.id)">
            {{info.name}}, {{info.sys.country}}
          </b-badge>
          
          <b-tooltip target="city">Click for extended forecast</b-tooltip>
          <img alt="Flag" :src="getImageUrl(info.sys.country)">
          {{info.weather[0].description}}
        </b-card-title>
        <b-card-text>
          <b-badge id="temperature" variant="info">{{`${temperatureRound(info.main.temp)} &#176;C`}}</b-badge>
          <b-tooltip target="temperature">Feels like: {{temperatureRound(info.main.feels_like)}}</b-tooltip>
          {{`temperature from ${info.main.temp_min} to ${info.main.temp_max} &#176;C, 
          wind ${info.wind.speed} m/s. clouds ${info.clouds.all} %`}}
          <br>
          Sunrise: {{timeFormat(info.sys.sunrise)}} Sunset: {{timeFormat(info.sys.sunset)}}
          <br>
          Geo coords {{`[${info.coord.lon}, ${info.coord.lat}]`}}
        </b-card-text>
      </b-card>
    </b-col>
  </b-row>
</template>

<script>
import moment from 'moment'
export default {
  props: [
    'info'
  ],
  methods: {
    getImageUrl(value) {
      return `http://openweathermap.org/images/flags/${value.toLowerCase()}.png`
    },
    getWeatherIconUrl(value) {
      return `http://openweathermap.org/img/w/${value}.png`
    },
    timeFormat (value) {
      return moment(value*1000).format('HH:mm')
    },
    temperatureRound (value) {
      return Math.round((value * 10) / 10).toFixed(1)
    }
  }
}
</script>
