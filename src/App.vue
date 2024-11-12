<script setup>
import { ref } from 'vue';
import SearchInpute from './components/SearchInpute.vue';
import WeatherCard from './components/WeatherCard.vue';
import Navbar from './components/Navbar.vue';

const places = ref([]);


const addPlace = (data) => {
  places.value.push(data)
}

const deletePlace = (name) => {
  places.value = places.value.filter(p => p.location.name !== name)
}
</script>

<template>
  <Navbar />
  <div class="text-center mb-6">


  </div>
  <div>
    <SearchInpute @places-data="addPlace" />
  </div>

  <div class="grid grid-cols-2 gap-4">
    <div v-for="(place, index) in places" :key="index">
      <WeatherCard :place="place" @delete-place="deletePlace" />
    </div>
  </div>
</template>

<style scoped></style>
