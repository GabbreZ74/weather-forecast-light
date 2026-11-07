<script setup lang="ts">
import { computed, ref } from "vue";
import { invoke } from "@tauri-apps/api/core";
import Sidebar from "./components/Sidebar.vue";
import WeatherView from "./components/WeatherView.vue";
import sunnyIcon from "./assets/basic-weather-forecast/Sunny.jpg.png";
import cloudyIcon from "./assets/basic-weather-forecast/Cloudy.png";
import rainyIcon from "./assets/day-wheather-forecast/Sun-and-rain.png";

// Reactive variables to store weather data
const mainCityWeather = ref<string | null>(null);
const mainCityHumidity = ref<number | null>(null);
const mainCityWind = ref<number | null>(null);
const mainCityPressure = ref<number | null>(null);
const mainCityTemp = ref<number | null>(null);
const mainCityFeelsLike = ref<number | null>(null);
const arrayOfDays = ref<unknown[]>([]);
const mainCityName = ref("");

const weatherIcons: Record<string, string> = {
  sunny: sunnyIcon,
  clear: sunnyIcon,
  cloudy: cloudyIcon,
  clouds: cloudyIcon,
  rainy: rainyIcon,
  rain: rainyIcon,
  drizzle: rainyIcon,
};
const weatherIcon = computed(
  () => weatherIcons[mainCityWeather.value ?? ""] ?? null
);

// Function to fetch weather data for a given city
const fetchMainWeatherData = async (city: string) => {
  try {
    const response = await invoke("get_weather_data", { city });
    const data = JSON.parse(response as string);

    // Update reactive variables with fetched data
    mainCityHumidity.value = data.humidity;
    mainCityWind.value = data.wind;
    mainCityPressure.value = data.pressure;
    mainCityTemp.value = data.temp;
    mainCityFeelsLike.value = data.feels_like;
    mainCityWeather.value = data.weather;
    arrayOfDays.value = data.arrayOfDays;
    mainCityName.value = city;
  } catch (error) {
    console.error("Error fetching weather data:", error);
  }
};

const fetchAllDaysWeather = async (city: string) => {
  try {
    const response = await invoke("get_all_days_weather", { city });
    arrayOfDays.value = JSON.parse(response as string);
  } catch (error) {
    console.error("Error fetching all days weather:", error);
  }
};

const handleCitySelected = async (city: string) => {
  await fetchMainWeatherData(city);
  await new Promise(resolve => setTimeout(resolve, 500));
  await fetchAllDaysWeather(city);
};

</script>


<template>
  <div class="app-layout">
    <Sidebar
      @citySelected="handleCitySelected"
    />
    <main class="main-content">
      <h1 class="app-title">Weather Forecast App</h1>
      <p>Welcome to the Weather Light Forecast App!</p>
      <WeatherView
        :weather-icon="weatherIcon"
        :main-city-name="mainCityName"
        :main-city-weather="mainCityWeather"
        :main-city-humidity="mainCityHumidity"
        :main-city-wind="mainCityWind"
        :main-city-pressure="mainCityPressure"
        :main-city-temp="mainCityTemp"
        :main-city-feels-like="mainCityFeelsLike"
        :array-of-days="arrayOfDays"
      />
    </main>
  </div>
</template>

//Style section

<style>

  .app-layout {
    display: flex;
    min-height: 100vh;
    border-radius: 10px;
    overflow: hidden;
  }

  .main-content {
    flex: 1;
    padding: 20px;
    background-size: cover;
    background-position: center;
    color: #fff;
  }

  .app-title {
    font-size: 30px;
    margin-bottom: 10px;
    font-weight: bold;
    font-family: 'Arial', sans-serif;
    font-style: italic;
  }

  .app-title:hover {
    color: #00c4f5;
    transition: color 0.3s ease;
    transform: scale(1.05);
  }
  .app-description {
    font-size: 18px;
    margin-bottom: 20px;
    font-family: 'Arial', sans-serif;
  }
  
  .app-description:hover {
    color: #00c4f5;
    transition: color 0.3s ease;
    transform: scale(1.05);
  }
  .notification-icon {
    width: 30px;
    height: 30px;
    cursor: pointer;
  }

  .main-weather-icon {
    width: 100px;
    height: 100px;
    margin-top: 20px;
    align-self: center;
  }

  .main-weather-icon:hover {
    transform: scale(1.1);
    transition: transform 0.4s ease;
    border-radius: 50px;
  }

</style>
