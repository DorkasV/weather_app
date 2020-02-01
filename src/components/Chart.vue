<script>
import { Line } from 'vue-chartjs'

export default {
  extends: Line,
  data: () => ({
    chartdata: {
      labels: [],
      datasets: [
        {
          label: 'Temperature, °C',
          backgroundColor: '#ffed83',
          data: []
        }
      ]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false
    },
    weatherData: {
      date: [],
      value: []
    }
  }),
  props: [
    'details'
  ],
  computed: {
    cityDetails(){
      if(this.details){
        return this.details
      }
      return null
    }
  },
  watch: {
    cityDetails () {
      if (this.cityDetails) {
        this.getData()
      }
      else {
        this.weatherData.date = []
        this.weatherData.value = []
        this.chartdata.labels = []
        this.chartdata.datasets[0].data = []
      }
    }
  },
  mounted() {
    if(this.cityDetails){
      this.getData()
    }
  },
  methods: {
    getData(){
      this.cityDetails.list.forEach(element => {
        if(this.weatherData.date.some(v => element.dt_txt.substring(0,10).includes(v.substring(0,10))))
        { 
          this.weatherData.date.push(element.dt_txt.substring(10,16))
        }
        else {
          this.weatherData.date.push(element.dt_txt.substring(0,16))
        }
        this.weatherData.value.push(element.main.temp)
      })
      this.chartdata.labels = this.weatherData.date
      this.chartdata.datasets[0].data = this.weatherData.value
      this.renderChart(this.chartdata, this.options)
    }
  },
}
</script>