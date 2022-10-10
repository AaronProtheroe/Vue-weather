<template>
  <div class="app">
    <div class="search_area">
      <input type="text" placeholder="Search city..." class="search_box" v-model='query'/>
      <span class="material-icons search_button" v-on:click="fetchWeather">search</span>
    </div>

    <div class="weather_container">
      <div class="no_search" v-if="place == ''">
        Type the name of a city to search
      </div>
      <div class="weather_card" v-if="place != ''">
        <div class="place">
          <p>{{place}}, {{country}}</p>
        </div>

        <div class="temp">
          <h1><span class="material-icons temp_icon">thermostat</span>{{Math.round(temp)}} °C</h1>
        </div>

        <div class="weather">
          <h5>Humidity: {{humidity}}%</h5>
          <h5>Pressure: {{pressure}} hPa</h5>
          <h5>Current weather: {{weather}}</h5>
        </div>
      </div>
    </div>
  </div>
</template>

<script>


export default {
  name: 'App',
  data() {
    return {
      api_key: 'd020d4294df30052d5120dfff8353fb1',
      base_url: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      place: '',
      country: '',
      temp: '',
      humidity: '',
      pressure: '',
      weather: '',
    }
  },
  methods: {
    
    // Method to fetch weather based on the city name from the user input
    fetchWeather() {
      fetch(`${this.base_url}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
      .then((res) => { 
      return res.json()}).then(this.setResults).then(this.clearSearchbar)
    }
    ,

    // Method to set the variables of weather parameters that will be displayed in the UI
    setResults(results) {
      this.place = results.name
      this.temp = results.main.temp
      this.humidity = results.main.humidity
      this.pressure = results.main.pressure
      this.country = results.sys.country
      this.weather = results.weather[0].description
    },

    // Method to clear the search bar after the user searches for a city
    clearSearchbar() {
      this.query = ''
    }
  }
  }
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: sans-serif;
  }
  .no_search{
    height: 80px;
    width: 400px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 25px;
    font-weight: 700;
  }
  .app {
    width: 100vw;
    height: 100vh;
    background-image: url('./assets/background.jpg');
    background-repeat: no-repeat;
    background-size: 100%;
  }
  .search_area {
    padding-top: 60px;
    display: flex;
    justify-content: center;
  }
  .search_box {
    width: 350px;
    height: 35px;
    border: none;
    padding-left: 15px;
    border-radius: 0px 16px 0px 16px;
    margin-right: 20px;
    transition: 0.7s;
  }
  .search_box:focus{
    border-radius: 16px 0px 16px 0px;
    transition: 0.7s;
    outline: none;
  }
  .search_button {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 35px;
    width: 65px;
    background-color: rgb(102, 102, 102);
    color: white;
    border: none;
    border-radius: 2rem;
    transition: 0.5s;
  }
  .search_button:hover{
    cursor: pointer;
    background-color: gray;
    transition: 0.5s;
  }

  .weather_container {
    margin-top: 60px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .weather_card {
    background-color: rgb(78, 78, 78);
    color: white;
    width: 500px;
    height: 360px;
    border-radius: 30px;
    opacity: 85%;
    transition: 0.7s;
  }
  .weather_card:hover{
    background-color: rgb(46, 46, 46);
    transition: 0.7s;
    scale: 1.05;
  }
  .place {
    margin-top: 30px;
    display: flex;
    justify-content: center;
  }
  .place p {
    font-weight: 10;
    font-size: 34px;
  }
  .temp {
    display: flex;
    justify-content: center;
    margin-top: 60px;
  }
  .temp h1{
    font-size: 46px;
    font-weight: 100;
  }
  .weather {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 45px;
  }
  .weather h5 {
    margin: 7px;
    font-size: 20px;
    font-weight: lighter;
  }

</style>
