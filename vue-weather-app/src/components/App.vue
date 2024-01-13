<!--App.vue-->

<template>
  <div>
    <input v-model="cityInput" placeholder="Enter City" @keyup.enter="fetchWeatherData">
    <button @click="fetchWeatherData">Get Weather</button>
    <button @click="toggleUnits">{{ unitLabel }}</button>
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
