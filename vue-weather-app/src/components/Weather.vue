<!--Weather.vue-->
  
<template>
  <div class="weather-container">
    <h2 class="weather-title">Weather for {{ cityName }}</h2>
    <div class="weather-info">
      <p class="temperature">Temperature: {{ temperature }}°C</p>
      <p class="description">Weather: {{ weatherDescription }}</p>
    </div>
    <div class="additional-info">
      <p>Humidity: {{ additionalInfo.humidity }}%</p>
      <p>Wind Speed: {{ additionalInfo.windSpeed }} km/h</p>
      <p>Sunrise: {{ formatTime(additionalInfo.sunrise) }}</p>
      <p>Sunset: {{ formatTime(additionalInfo.sunset) }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'WeatherReport',
  props: {
    cityName: String,
    temperature: Number,
    weatherDescription: String,
    additionalInfo: Object,
  },
  methods: {
    formatTime(timestamp) {
      let date = new Date(timestamp * 1000);
      return date.toLocaleTimeString();
    },
  },
};
</script>

<style scoped>
.weather-container {
  width: 350px; /* Adjusted for a smaller size */
  height: 420px; /* Adjusted for a smaller size */
  margin: auto; /* Center the box */
  border-radius: 10px;
  background: #8698EF;
  box-shadow: 0 4px 10px 0 rgba(0, 0, 0, 0.25);
  color: #FFF;
  font-family: 'Source Serif Pro';
  font-size: 24px; /* Reduced for better readability */
  font-weight: 600;
  padding: 20px; /* Added padding for internal spacing */
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.weather-title {
  margin-bottom: 15px;
}

.weather-info, .additional-info {
  font-size: 20px; /* Reduced for better readability */
  font-weight: 400;
  margin-bottom: 10px; /* Added margin for spacing */
}
</style>
