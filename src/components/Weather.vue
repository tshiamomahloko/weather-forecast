<template>
  <div class="container">
    <div class="search">
      <h1 class="heading">Weather Forecast</h1>
      <md-field>
        <label>Enter City...</label>
        <md-input v-model="query" @keypress="fetchWeather"></md-input>
        <span class="md-helper-text">eg: Pretoria</span>
      </md-field>
    </div>
    <div class="elevation-demo">
      <md-content
        class="md-elevation-24"
        v-if="weather.main != null || weather.main != undefined"
      >
        <h1 class="city">{{ weather.name }}, {{ weather.sys.country }}</h1>
        <h3 class="date">{{ date }}</h3>
        <h1 class="temp">{{ weather.main.temp }}°</h1>
      </md-content>
      <md-content class="md-elevation-24" v-if="weather.cod === '404'">
        <h1 class="not-found">{{ none }}</h1>
      </md-content>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Weather',
    data() {
      return {
        api_key: '256b5796e8504ef0369cd2a82933c745',
        url_base: 'https://api.openweathermap.org/data/2.5/',
        query: null,
        weather: {},
        date: new Date(),
        none: 'City Not Found'
      }
    },
    methods: {
      fetchWeather(e) {
        if (e.key == 'Enter') {
          fetch(
            `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
          )
            .then((res) => {
              return res.json()
            })
            .then(this.results)
        }
      },
      results(res) {
        this.weather = res
      }
    }
  }
</script>

<style scoped>
  .container {
    padding: 2rem;
  }
  .search {
    position: fixed;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 21rem;
    padding-top: 2rem;
  }

  .elevation-demo {
    padding-top: 10%;
    display: flex;
    align-content: center;
    text-align: center;
    position: fixed;
    left: 50%;
    transform: translate(-50%, -5%);
  }
  .md-elevation-24 {
    text-align: center;
    border: black 2px;
    border-radius: 2rem;
    padding: 1rem;
  }
</style>
