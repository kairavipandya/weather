<!--App.vue-->

<template>
  <div class="app-container">
    <div class="search-area">
      <input
        v-model="cityInput"
        placeholder="Search for a city..."
        @keyup.enter="addCity"
        class="search-bar"
      />
      <button @click="toggleUnits" class="unit-switch">{{ unitLabel }}</button>
    </div>
    
    <div class="weather-cards">
      <div v-for="(cityWeather, index) in cityWeatherData" :key="index" class="weather-card">
        <Weather
          :cityName="cityWeather.name"
          :temperature="cityWeather.temp"
          :weatherDescription="cityWeather.description"
          :additionalInfo="cityWeather.additionalInfo"
          :icon="getWeatherIcon(cityWeather.description)"
        />
      </div>
    </div>
  </div>
</template>

<script>
import Weather from "./Weather.vue";
import axios from 'axios';
import { ref, computed } from 'vue';

export default {
  components: {
    Weather,
  },
  setup() {
    const cityInput = ref("");
    const cityWeatherData = ref([]);
    const isMetric = ref(true);

    const addCity = async () => {
      if (cityInput.value.length > 2) {
        const cityExists = cityWeatherData.value.some(city => city.name.toLowerCase() === cityInput.value.toLowerCase());
        if (!cityExists) {
          const apiKey = "091f6b8e5739002fb142ecf079bd7fd8";
          const units = isMetric.value ? 'metric' : 'imperial';
          const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${cityInput.value}&units=${units}&appid=${apiKey}`;

          try {
            const response = await axios.get(apiUrl);
            const { main, weather, wind, sys } = response.data;

            cityWeatherData.value.push({
              name: response.data.name,
              temp: main.temp,
              description: weather[0].description,
              additionalInfo: { humidity: main.humidity, windSpeed: wind.speed, sunrise: sys.sunrise, sunset: sys.sunset },
              icon: getWeatherIcon(weather[0].description)
            });

            cityInput.value = "";
          } catch (error) {
            console.error("Error fetching weather data:", error);
          }
        }
      }
    };

    const toggleUnits = async () => {
      isMetric.value = !isMetric.value;
      const units = isMetric.value ? 'metric' : 'imperial';

      await Promise.all(cityWeatherData.value.map(cityWeather => updateCityWeather(cityWeather, units)));
    };

    const updateCityWeather = async (cityWeather, units) => {
      const apiKey = "091f6b8e5739002fb142ecf079bd7fd8";
      const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${cityWeather.name}&units=${units}&appid=${apiKey}`;

      try {
        const response = await axios.get(apiUrl);
        cityWeather.temp = response.data.main.temp;
      } catch (error) {
        console.error("Error fetching weather data:", error);
      }
    };

    const unitLabel = computed(() => isMetric.value ? 'Switch to °F' : 'Switch to °C');

    const getWeatherIcon = (description) => {
    const lowerCaseDescription = description.toLowerCase();
    switch (lowerCaseDescription) {
    case 'clear sky':
      return 'fas fa-sun'; // Sun icon for clear sky
    case 'few clouds':
      return 'fas fa-cloud-sun'; // Cloud-sun icon for few clouds
    case 'scattered clouds':
    case 'broken clouds':
      return 'fas fa-cloud'; // Cloud icon for scattered or broken clouds
    case 'shower rain':
    case 'rain':
      return 'fas fa-cloud-rain'; // Cloud-rain icon for rain
    case 'thunderstorm':
      return 'fas fa-bolt'; // Bolt icon for thunderstorm
    case 'snow':
      return 'fas fa-snowflake'; // Snowflake icon for snow
    case 'mist':
      return 'fas fa-smog'; // Smog icon for mist
    default:
      return 'fas fa-meteor'; // Meteor icon as a default
  }
};


    return {
      cityInput,
      cityWeatherData,
      toggleUnits,
      unitLabel,
      getWeatherIcon,
      addCity,
    };
  },
};
</script>


<style>
.app-container {
  background: #ACB7EF;
  padding: 20px;
  min-height: 100vh;
}

.search-area {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.weather-cards {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}

.weather-card {
  margin: 10px;
}

.remove-city {
  background-color: #FF6B6B;
  color: white;
  border: none;
  border-radius: 5px;
  padding: 5px 10px;
  cursor: pointer;
}
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
