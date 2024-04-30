<template>
  <div class="wrapper">
    <h1>Weather app</h1>
    <h3>Find out the weather in {{ !this.city && '...' || getCity }}</h3>
    <input type="text" v-model="this.city" placeholder="Enter the city">
    <button v-show="this.city" @click="getWeather()">Get weather</button>
    <p class="error">{{ this.error }}</p>

    <div class="temp" v-if="this.info">
      <p>{{ getTemp }}</p>
      <p>{{ getTempFeelsLike }}</p>
      <p>{{ getTempMin }}</p>
      <p>{{ getTempMax }}</p>
    </div>

  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return { city: '', error: '', info: '' }
  },
  computed: {
    getCity() { return `«${this.city}»` },
    getTemp() { return `Temp: ${this.info.main.temp}` },
    getTempFeelsLike() { return `Feels like: ${this.info.main.feels_like}` },
    getTempMin() { return `Min temp: ${this.info.main.temp_min}` },
    getTempMax() { return `Max temp: ${this.info.main.temp_max}` }
  },
  methods: {
    getWeather() {
      this.error = this.city.trim().length > 2 ? '' : 'Insufficient number of characters'
      axios.get(`http://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=3d9de74844d28377e81415151cbe6a66`)
        .then(res => (this.info = res.data))
        .catch(err => console.error(err))
    }
  }
}
</script>

<style scoped>
.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background-color: beige;
  text-align: center;
}

.wrapper h1 {
  margin-top: 30px;
}

.wrapper h3 {
  font-style: oblique;
}

.wrapper input {
  margin-top: 10px;
  background-color: transparent;
  border: 0;
  border-bottom: 2px solid black;
  font-style: oblique;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:hover,
input:focus {
  border-bottom-color: gray;
}

.wrapper button {
  background-color: black;
  color: rgb(169, 169, 169);
  border-radius: 10px;
  border: 0px;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
  font-style: oblique;
  font-size: 16px;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-2px);
}

.error {
  margin-top: 20px;
  color: red;
}

.temp p {
  margin-top: 50px;
  font-size: 24px;
}
</style>