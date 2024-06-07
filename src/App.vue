<template>
  <div id="app">
    <h1>Hava Durumu Uygulaması</h1>
    <input v-model="city" placeholder="Şehir adı girin" />
    <button @click="getWeather">Hava Durumunu Getir</button>
    <div v-if="weather">
      <h2>{{ weather.name }}</h2>
      <img :src="getWeatherIconUrl(weather.weather[0].icon)" alt="Weather Icon">
      <p>{{ weather.weather[0].description }}</p>
      <p>Rüzgar: {{ formatWind(weather.wind.speed) }} m/s</p>
      <p>Sıcaklık: {{ formatTemperature(weather.main.temp) }}°C</p>
      <p>Nem: {{ weather.main.humidity }}%</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      city: '', // Giriş alanı boş kalacak
      weather: null,
    };
  },
  mounted() {
    if (!this.city) {
      this.city = ''; // Eğer city verisi boşsa, boş string olarak bırakıyoruz
      this.getWeather(); // Bileşen yüklendiğinde hava durumu bilgilerini almak için getWeather metodunu çağırıyoruz
    }
  },
  methods: {
    async getWeather() {
      const cityName = this.city || 'İzmir'; // Eğer city verisi boşsa, İzmir'i varsayılan olarak kullanıyoruz
      const apiKey = 'bd5e378503939ddaee76f12ad7a97608';
      const url = `https://api.openweathermap.org/data/2.5/weather?q=${cityName}&units=metric&lang=tr&appid=${apiKey}`;
      try {
        const response = await axios.get(url);
        this.weather = response.data;
      } catch (error) {
        console.error(error);
        alert('Hava durumu bilgileri alınamadı. Lütfen tekrar deneyin.');
      }
    },
    getWeatherIconUrl(iconCode) {
      return `https://openweathermap.org/img/wn/${iconCode}.png`;
    },
    formatTemperature(temperature) {
      return temperature.toFixed(1);
    },
    formatWind(windSpeed) {
      return windSpeed.toFixed(1);
    },
  },
};
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  margin-top: 60px;
}
input {
  padding: 8px;
  margin-right: 8px;
}
button {
  padding: 8px;
}
h2 {
  margin-bottom: 0px;
}
p {
  margin-top: 0px;
}
</style>
