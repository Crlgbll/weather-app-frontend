<template>
  <div
    class="h-screen w-full bg-black text-white p-5 flex flex-col justify-center items-center gap-4"
  >
    <h1 class="font-bold text-5xl">Simple Weather Application</h1>
    <div
      class="h-4/5 w-[1200px] p-5 rounded-lg flex flex-col border-2 border-gray-800 items-center"
    >
      <div class="p-4 gap-2 flex">
        <input
          v-model="place"
          type="text"
          placeholder="Enter a place"
          class="h-10 w-[400px] bg-transparent border-2 border-gray-800 rounded-xl text-2xl p-2"
        />
        <button
          class="h-10 w-24 bg-black hover:bg-white hover:text-black border-2 border-gray-800 rounded-xl"
          @click="searchPlace"
        >
          Search <icon name="ic:round-search" />
        </button>
      </div>
      <div
        v-if="place && weather.weather.weather"
        class="h-full w-[1000] border-2 border-gray-800 rounded-lg flex justify-around items-center"
      >
        <div class="w-[400px] h-full p-5">
          <img
            class="h-96 w-96 animate-pulse"
            :src="iconUrl"
            alt="Weather Icon"
          />
          <h1 class="text-5xl">
            {{ weather.place + "," + weather.weather.sys.country }}
          </h1>
          <p class="text-8xl">{{ weather.weather.main.temp }}°C</p>
        </div>
        <div class="h-96 w-96 flex flex-col justify-center items-center gap-5">
          <!-- You can uncomment and add more weather details here -->
          <h1 class="text-3xl font-bold">Meteorological Data</h1>
          <div class="flex flex-row gap-5">
            <div class="flex flex-col w-36 border-2 text-center">
              <h1 class="text-xl">
                <icon name="lets-icons:temperature" /> Feels like
              </h1>
              <p class="text-3xl font-bold">
                {{ weather.weather.main.feels_like }}°
              </p>
            </div>
            <div class="flex flex-col w-36 border-2 text-center">
              <h1 class="text-xl">
                <icon name="material-symbols:humidity-mid" /> Humidity
              </h1>
              <p class="text-3xl font-bold">
                {{ weather.weather.main.humidity }}
              </p>
            </div>
          </div>
          <div class="flex flex-row gap-5">
            <div class="flex flex-col w-36 border-2 text-center">
              <h1 class="text-xl">
                <icon
                  name="streamline:interface-weather-temperature-cold-temperature-thermometer-minus-mercury-cold-weather"
                />
                Min Temp
              </h1>
              <p class="text-3xl font-bold">
                {{ weather.weather.main.temp_min }}
              </p>
            </div>
            <div class="flex flex-col w-36 border-2 text-center">
              <h1 class="text-xl"><icon name="mdi:weather-windy" /> Wind</h1>
              <p class="text-3xl font-bold">
                {{ weather.weather.wind.speed }}
              </p>
            </div>
          </div>
          <div class="flex flex-row gap-5">
            <div class="flex flex-col w-36 border-2 text-center">
              <h1 class="text-xl">
                <icon
                  name="streamline:interface-weather-temperature-hot-temperature-thermometer-hot-mercury-plus-weather"
                />
                Max Temp
              </h1>
              <p class="text-3xl font-bold">
                {{ weather.weather.main.pressure }}
              </p>
            </div>

            <div class="flex flex-col w-36 border-2 text-center">
              <h1 class="text-xl">
                <icon name="lets-icons:pressure" /> Pressure
              </h1>
              <p class="text-3xl font-bold">
                {{ weather.weather.main.pressure }}
              </p>
            </div>
          </div>
        </div>
      </div>
      <!-- <div>Temperature: {{ weatherPlace.weather.main.temp }}°C</div>
      <div>Feels Like: {{ weatherPlace.weather.main.feels_like }}°C</div> -->
    </div>
  </div>
</template>

<script setup>
let place = ref("");
const weather = ref(null); // Initialize as null
const iconUrl = ref("");
const isLoading = ref(false); // To handle loading state
const noData = ref(true); // To handle no data state

const searchPlace = async () => {
  if (!place.value) {
    // If no place is entered, don't do anything
    noData.value = true;
    return;
  }
  isLoading.value = true;
  const { data: response, error } = await useFetch(
    `http://weather-app-backend.test/api/weather/${place.value}`,
    {}
  );
  isLoading.value = false;

  if (!response.value || error.value) {
    // Handle error or no data
    noData.value = true;
    weather.value = null; // Ensure weather is set to null if there's an error or no data
  } else {
    noData.value = false;
    weather.value = response.value;
    const iconCode = weather.value.weather.weather[0].icon;
    iconUrl.value = `https://openweathermap.org/img/wn/${iconCode}@2x.png`;
  }
};
watch(place, (newPlace) => {
  if (!newPlace) {
    weather.value = null;
    noData.value = true;
  }
});
</script>
