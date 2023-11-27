<template>
  <div>
    <img :src="iconUrl" alt="Weather Icon" />
    <!-- You can add more weather details here if you like -->
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

const iconUrl = ref("");

// Function to fetch weather data
async function fetchWeatherData() {
  const response = await fetch(
    "https://api.openweathermap.org/data/2.5/weather?q=London&appid=your_api_key"
  );
  const data = await response.json();
  updateWeatherIcon(data);
}

// Function to update the weather icon
function updateWeatherIcon(data) {
  const iconCode = data.weather[0].icon;
  iconUrl.value = `https://openweathermap.org/img/wn/${iconCode}@2x.png`;
}

onMounted(() => {
  fetchWeatherData();
});
</script>
