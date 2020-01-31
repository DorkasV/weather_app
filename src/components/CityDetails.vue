<template>
  <div>
    <b-row v-for="(itemm, key, index) in results" :key="index" align-h="center">
      <b-col lg="6">
        <b-card
          tag="article"
          class="mb-2"
        >
          <b-card-title>
            {{key}}
          </b-card-title>
          <b-list-group flush v-for="(item, indexx) in itemm" :key="indexx">
            <b-list-group-item>
              <b-row>
                <b-col lg="4" class="align-left">
                  {{timeFormat(item.dt_txt)}}
                  <img alt="Flag" :src="getWeatherIconUrl(item.weather[0].icon)">
                </b-col>
                <b-col lg="8">
                  <b-row>
                    <b-col md="2">
                      <b-badge :variant="temperature(item.main.temp)">{{Math.round(item.main.temp)}} &#176;C</b-badge>
                    </b-col>
                    <b-col md="10">
                      {{item.weather[0].description}}
                    </b-col>
                  </b-row>
                  <b-row>
                    <b-col offset-md="2">
                      {{item.wind.speed}} m/s
                    </b-col>
                  </b-row>
                  <b-row>
                    <b-col offset-md="2">
                      clouds: {{item.clouds.all}} %
                    </b-col>
                  </b-row>
                </b-col>
              </b-row>
            </b-list-group-item>
          </b-list-group>
        </b-card>
      </b-col>
    </b-row>
  </div>
</template>

<script>
import moment from 'moment'
import _ from 'lodash'
export default {
  data() {
    return {
      results: null,
    }
  },
  props: [
    'details'
  ],
  watch: {
    details() {
      if (this.details) {
        const day = item => moment(item.dt_txt).format('YYYY-MM-DD')
        this.results = _.groupBy(this.details.list, day)
      }
      else {
        this.results = null
      }
    }
  },
  methods: {
    getWeatherIconUrl (value) {
      return `http://openweathermap.org/img/w/${value}.png`
    },
    timeFormat (value) {
      return moment(value).format('HH:mm')
    },
    temperature (value) {
      if (value < 0)
        return 'info'
      return 'warning'
    }
  }
}
</script>
