<template>
  <div class="user-card">
    <div class="user-card__header">
      <h2>{{ user.name  }} <small>{{   weatherData.elapsedTime +"ms"}}</small></h2>
      <div class="user-card__location">

        <span>{{ weatherData.temperature }}°C</span>
      </div>
    </div>
    <div class="user-card__body">
      <div class="user-card__weather">
        <img :src="weatherIcon" :alt="weatherData.description">
        <span>{{ weatherData.description }}</span>
      </div>
      <div class="user-card__details">
        <div class="user-card__detail">
          <span>Humidity:</span>
          <span>{{ weatherData.humidity }}%</span>
        </div>
        <div class="user-card__detail">
          <span>Wind:</span>
          <span>{{ weatherData.windSpeed }} km/h</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'UserCard',
  props: {
    user: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      weatherData: {},
    };
  },
  computed: {
    weatherIcon() {
      if (!this.weatherData.icon) return '';
      return `https://openweathermap.org/img/w/${this.weatherData.icon}.png`;
    },
  },
  methods: {
    async getWeatherData() {
      const { latitude, longitude } = this.user;
      const API_KEY = 'd601f76e5cf1e1040de23860c924d972';
      const url = `https://api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&appid=${API_KEY}&units=metric`;
      const startTime = new Date().getTime();
      const response = await axios.get(url);
      const endTime = new Date().getTime();
      const elapsedTime = endTime - startTime;
      // if (elapsedTime > 500) {
            console.log('Fetch weather request for User ID:'+this.user.id +" take : "+elapsedTime +"ms");
      // }
      const { main, weather, wind } = response.data;
      this.weatherData = {
        temperature: main.temp,
        humidity: main.humidity,
        windSpeed: wind.speed,
        description: weather[0].description,
        icon: weather[0].icon,
        elapsedTime:elapsedTime
     };
    },
  },
  mounted() {
    this.getWeatherData();
  },
};
</script>

<style scoped>
.user-card {
  border: 1px solid #ccc;
}
</style>