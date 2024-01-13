<!--App.vue-->

<template>
  <div>
    <input
      v-model="cityInput"
      placeholder="Search for a city..."
      @keyup.enter="fetchWeatherData"
      class="search-bar"
    />
    <button @click="toggleUnits" class="unit-switch">{{ unitLabel }}</button>
    <Weather
      :cityName="cityName"
      :temperature="temperature"
      :weatherDescription="weatherDescription"
      :additionalInfo="additionalInfo"
    />
  </div>
</template>

<script>
import Weather from "./Weather.vue";
import axios from 'axios';
import { ref, onMounted, watch, computed } from 'vue';

export default {
  components: {
    Weather,
  },
  setup() {
    const cityInput = ref("New York");
    const cityName = ref("");
    const temperature = ref("");
    const weatherDescription = ref("");
    const additionalInfo = ref({});
    const isMetric = ref(true);

    const fetchWeatherData = async () => {
      const apiKey = "091f6b8e5739002fb142ecf079bd7fd8";
      const units = isMetric.value ? 'metric' : 'imperial';
      const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${cityInput.value}&units=${units}&appid=${apiKey}`;

      try {
        const response = await axios.get(apiUrl);
        const { main, weather, wind, sys } = response.data;

        cityName.value = response.data.name;
        temperature.value = main.temp;
        weatherDescription.value = weather[0].description;
        additionalInfo.value = { humidity: main.humidity, windSpeed: wind.speed, sunrise: sys.sunrise, sunset: sys.sunset };
      } catch (error) {
        console.error("Error fetching weather data:", error);
      }
    };

    const toggleUnits = () => {
      isMetric.value = !isMetric.value;
      fetchWeatherData();
    };

    const unitLabel = computed(() => isMetric.value ? 'Switch to °F' : 'Switch to °C');

    watch(() => cityInput.value, fetchWeatherData);

    onMounted(fetchWeatherData);

    return {
      cityInput,
      cityName,
      temperature,
      weatherDescription,
      additionalInfo,
      toggleUnits,
      unitLabel,
    };
  },
};
</script>

<style>
.search-bar {
  width: 600px;
  height: 43px;
  flex-shrink: 0;
  border-radius: 20px;
  border: 1px solid #000;
  box-shadow: 0 4px 10px 0 rgba(0, 0, 0, 0.25);
  color: #000;
  font-family: 'Source Serif Pro';
  font-size: 24px;
  background: #F5F5F5;
  margin-bottom: 20px; /* Added spacing */
  margin-right: 20px;

}

.unit-switch {
  width: 164px;
  height: 49px;
  flex-shrink: 0;
  border-radius: 20px;
  background: #8698EF;
  box-shadow: 0 4px 10px 0 rgba(0, 0, 0, 0.25);
  color: #FFF;
  font-family: 'Source Serif Pro';
  font-size: 24px;
  margin-bottom: 20px; /* Added spacing */
}
</style>
