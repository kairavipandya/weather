<!--App.vue-->

<template>
  <div>
    <Weather
      :cityName="cityName"
      :temperature="temperature"
      :weatherDescription="weatherDescription"
    />
  </div>
</template>

<script>
import Weather from "./Weather.vue";
import axios from 'axios';
import { ref, onMounted } from 'vue';

export default {
  components: {
    Weather,
  },
  setup() {
    const cityName = ref("");
    const temperature = ref("");
    const weatherDescription = ref("");

    const fetchWeatherData = async () => {
      const apiKey = "091f6b8e5739002fb142ecf079bd7fd8";
      const city = "New York"; // e.g., 'New York'
      const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${apiKey}`;

      try {
        const response = await axios.get(apiUrl);
        const { main, weather, name } = response.data;

        console.log("Response data:", response.data); // Check the response data in the console

        cityName.value = name;
        temperature.value = main.temp;
        weatherDescription.value = weather[0].description;
      } catch (error) {
        console.error("Error fetching weather data:", error);
      }
    };

    onMounted(() => {
      fetchWeatherData();
    });

    return {
      cityName,
      temperature,
      weatherDescription,
    };
  },
};
</script>

