<template>
  <b-row v-if="info" align-h="center">
    <b-col cols="4">
      <b-card
        :title="`${(info||{}).name}, ${((info||{}).sys||{}).country} ${((info||{}).weather||{})[0].description}`"
        tag="article"
        style="max-width: 30rem;"
        class="mb-2"
        @click="$emit('cityDetails', info.id)"
      >
        <b-card-text>
          <img alt="Flag" :src="getImageUrl(info.sys.country)">
          <b-badge variant="info">{{`${((info||{}).main||{}).temp} &#176;C`}}</b-badge>
          {{`temperature from ${((info||{}).main||{}).temp_min} to ${((info||{}).main||{}).temp_max} &#176;C, 
          wind ${((info||{}).wind||{}).speed} m/s. clouds ${((info||{}).clouds||{}).all} %`}}
          <br>
          Geo coords {{`[${((info||{}).coord||{}).lon}, ${((info||{}).coord||{}).lat}]`}}
        </b-card-text>
      </b-card>
      <b-button variant="info" @click="$emit('cityDetails', info.id)">5 day / 3 hour weather forecast
</b-button>
    </b-col>
  </b-row>
</template>

<script>
export default {
  props: [
    'info'
  ],
  methods: {
    getImageUrl(value) {
      return `http://openweathermap.org/images/flags/${value.toLowerCase()}.png`
    }
  }
}
</script>