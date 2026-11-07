<script setup lang="ts">
defineProps<{
    mainCityName: string;
    mainCityWeather: string | null;
    mainCityHumidity: number | null;
    mainCityWind: number | null;
    mainCityPressure: number | null;
    mainCityTemp: number | null;
    mainCityFeelsLike: number | null;
    arrayOfDays: unknown[] | null;
    weatherIcon: string | null;
}>();
</script>

<template>
    <section class="weather-card">
      <h2 class="weather-card-title">Current Weather in {{ mainCityName }}</h2>
      <img
        v-if="weatherIcon"
        class="weather-icon"
        :src="weatherIcon"
        alt="Weather condition"
      />
      <p v-if="mainCityTemp !== null">Temperature: {{ mainCityTemp }}°</p>
      <p v-if="mainCityFeelsLike !== null">Feels Like: {{ mainCityFeelsLike }}°</p>
      <p v-if="mainCityHumidity !== null">Humidity: {{ mainCityHumidity }}%</p>
      <p v-if="mainCityWind !== null">Wind Speed: {{ mainCityWind }} m/s</p>
      <p v-if="mainCityPressure !== null">Pressure: {{ mainCityPressure }}</p>
      <p v-if="mainCityWeather">Weather: {{ mainCityWeather }}</p>
      <p v-if="arrayOfDays && arrayOfDays.length > 0">Forecast for the next days:</p>
    </section>
</template>

<style scoped>
.weather-card {
    background-color: #cbc0c0;
    padding: 30px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    margin: 10px;
    text-align: center;
}

.weather-icon {
    display: block;
    width: 140px;
    height: 140px;
    margin: 0 auto 16px;
    object-fit: contain;
}

.weather-card:hover {
    transform: scale(1.05);
    transition: transform 0.2s ease;
}
</style>