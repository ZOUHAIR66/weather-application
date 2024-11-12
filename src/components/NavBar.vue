<template>
  <div v-if="search.res.location" class="container bg-grey-lightest mx-auto shadow rounded pb-4 bg-cover"
    style="color:#606F7B; background-color: rgb(165, 182, 198); background-image:url('https://68.media.tumblr.com/f6a4004f3092b0d664daeabb95d5d195/tumblr_oduyciOJNb1uhjffgo1_500.gif');">
    <div class="mt-2 p-4 border-b border-grey-light text-center">
      <span class="text-4xl font-thin">{{ search.res.location.name }}</span>
    </div>
    <div class="text-center text-xl text-grey-light p-4">
      <span>{{ search.res.current.condition.text }}</span>
    </div>
    <div class="flex justify-center">
      <div class="text-center p-2">
        <div class="text-lg text-grey-light">
          <span class="text-right">{{ search.res.current.temp_f }}˚F</span>
          <span class="text-center text-5xl text-white mx-6 font-thin">{{ Math.round(search.res.current.temp_c)
            }}˚C</span>
          <span class="text-left">{{ search.res.current.feelslike_f }}˚F</span>
        </div>
        <div class="text-grey-light tracking-wide">
          {{ search.res.location.localtime }}
        </div>
      </div>
    </div>
  </div>
  <div v-else>
    <p>Loading weather data...</p>
  </div>
</template>

<script>
import { onMounted, reactive } from 'vue';

export default {
  setup() {
    const search = reactive({
      res: {}  // Array to store weather data
    });

    const getWeather = async () => {
      try {
        const res = await fetch(`http://api.weatherapi.com/v1/current.json?key=0c6b100ecc7144f8a67172939240811&q=casablanca&aqi=no`);



        const data = await res.json();
        search.res = data;  // Store the forecast data in reactive state
        console.log(search);  // Optional: log the result to verify

      } catch (error) {
        console.error("Error fetching weather data:", error);
      }
    };

    onMounted(() => {
      getWeather();
    });

    return {
      search
    };
  }
};
</script>

<style scoped>
/* Your scoped CSS here */
</style>
