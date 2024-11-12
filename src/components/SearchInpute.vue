<template>
  <div>
    <!-- search field -->
    <form>
      <div class="bg-white border border-indigo-600/30 rounded-lg shadow-lg flex items-center">
        <i class="fa-solid fa-magnifying-glass p-2 text-indigo-600"></i>
        <input type="text" placeholder="Search for a place"
          class="rounded-r-lg p-2 border-0 outline-0 focus:ring-2 focus:ring-indigo-600 ring-inset w-full"
          v-model="searchTerm.query" @input="handleSearch" />
      </div>
    </form>
    <!-- search suggestions -->
    <div class="bg-white my-2 rounded-lg shadow-lg">
      <div v-if="searchTerm.query !== null">
        <div v-for="place in searchTerm.result" :key="place.id">
          <button class="px-3 my-2 hover:text-indigo-600 hover:font-bold w-full text-left"
            @click="getWeather(place.name)">
            {{ place.name }}
            {{ place.region }}
            {{ place.country }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>


<script setup>
import { reactive } from 'vue';

const emit = defineEmits('places-data')

const searchTerm = reactive({
  query: '',
  timeout: null,
  result: []
});

const handleSearch = () => {
  // Clear any existing timeout to debounce the function
  if (searchTerm.timeout) {
    clearTimeout(searchTerm.timeout);
  }

  // Set a new timeout
  searchTerm.timeout = setTimeout(async () => {
    try {
      // Proceed only if there’s a query
      if (searchTerm.query !== '') {
        const response = await fetch(`http://api.weatherapi.com/v1/current.json?key=0c6b100ecc7144f8a67172939240811&q=${searchTerm.query}&aqi=no`);

        // Check if the response is okay
        if (!response.ok) {
          throw new Error(`Error: ${response.statusText}`);
        }

        const data = await response.json();
        searchTerm.result = data; // Assign the data to searchTerm.result
        console.log(searchTerm.result);
      } else {
        searchTerm.result = null;
      }
    } catch (error) {
      console.error("Error fetching weather data:", error);
      searchTerm.result = { error: error.message };
    }
  }, 500); // Wait 500ms before executing the search
};

const getWeather = async (name) => {
  try {
    const res = await fetch(`http://api.weatherapi.com/v1/forecast.json?key=0c6b100ecc7144f8a67172939240811&q=${name}&days=3&aqi=no&alerts=no`);

    if (!res.ok) {
      throw new Error(`Error: ${res.statusText}`);
    }

    const data = await res.json();
    console.log(data.forecast.forecastday);
    emit('places-data', data);
  } catch (error) {
    console.error("Error fetching weather data:", error);
  }
};

</script>

<style lang="scss" scoped></style>
