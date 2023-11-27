<template>
  <div
    class="h-screen w-full bg-black text-white p-5 flex justify-center items-center"
  >
    <div
      class="h-4/5 w-11/12 p-5 rounded-lg flex flex-col border-2 border-gray-800"
    >
      <div class="p-4 gap-2 flex">
        <input
          v-model="place"
          type="text"
          class="h-10 w-[4d00px] bg-transparent border-2 border-gray-800 rounded-xl text-2xl"
        />
        <button
          class="h-10 w-24 bg-black hover:bg-white hover:text-black border-2 border-gray-800 rounded-xl"
          @click="searchPlace"
        >
          Search <icon name="ic:round-search" />
        </button>
      </div>
      <div
        class="h-full w-full border-2 border-gray-800 rounded-lg flex items-center justify-around"
      >
        <div
          class="h-96 w-96 border-2 border-gray-800 flex flex-col justify-center"
        >
          <img :src="iconUrl" alt="Weather Icon" />
        </div>
      </div>
      <!-- <div>Temperature: {{ weatherPlace.weather.main.temp }}°C</div>
        <div>Feels Like: {{ weatherPlace.weather.main.feels_like }}°C</div> -->
    </div>
  </div>
</template>

<script setup>
let place = ref("");
const weather = ref({
  weather: {
    coord: {},
    main: {},
    wind: {},
    clouds: {},
  },
});
const iconUrl = ref("");
const searchPlace = async () => {
  const { data: response, error } = await useFetch(
    `http://weather-app-backend.test/api/weather/${place.value}`,
    {}
  );

  if (!response.value) {
    error;
  } else {
    weather.value = response.value;
    const iconCode = weather.value.weather.weather[0].icon;
    iconUrl.value = `https://openweathermap.org/img/wn/${iconCode}@2x.png`;
    console.log(weather.value.weather.weather[0].main);
  }
};
// https://api.openweathermap.org/data/2.5/weather?q=London,GB&appid=f42a9c26e168276abe9f59ee93b758ff
</script>
