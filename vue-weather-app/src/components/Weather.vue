<!--Weather.vue-->

<template>
    <div class="weather-container">
      <h2 class="weather-title">Weather for {{ cityName }}</h2>
      <div class="weather-info">
        <p class="temperature">Temperature: {{ temperature }}°C</p>
        <p class="description">Weather: {{ weatherDescription }}</p>
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
    },
  };
  </script>
  
  <style scoped>
  .weather-container {
    background-color: #f4f4f4;
    border: 1px solid #ccc;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    text-align: center;
  }
  
  .weather-title {
    font-size: 24px;
    color: #333;
    margin-bottom: 10px;
  }
  
  .weather-info {
    font-size: 18px;
    color: #555;
  }
  
  .temperature {
    margin: 5px 0;
  }
  
  .description {
    margin: 5px 0;
  }
  </style>
  