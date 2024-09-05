<template>
  <div class="flex">
    <!-- Sidebar -->
    <div class="sidebar p-4 w-96 h-[100vh] overflow-y-scroll bg-gray-800 text-white">
      <h2 class="text-xl font-bold mb-4">Cities List</h2>
      <!-- Search bar -->
      <input
        type="text"
        v-model="searchText"
        placeholder="Search city..."
        class="w-full p-2 mb-4 rounded border-gray-300 focus:border-indigo-500 focus:ring focus:ring-indigo-200 focus:ring-opacity-50 text-black"
      />
      <ul v-if="cityNames.length > 0">
        <li
          v-for="city in filteredCities"
          :key="city.id"
          class="py-1 cursor-pointer hover:bg-gray-700"
          @click="selectCity(city)"
        >
          {{ city.name }}
        </li>
      </ul>
      <p v-else>Loading or no data available...</p>
    </div>

    <!-- Weather Details -->
    <div :style="{ backgroundImage: `url(${weatherBackground} )` }" class="flex-1 p-4 bg-gray-600 flex justify-center items-center flex-col bg-cover bg-center">
      <div style="background-color: rgba(0, 255, 200, 0.674); " class='p-6 rounded-md'>
          <h2 class="text-3xl text-white font-bold mb-4">Weather Details</h2>
          <div v-if="selectedCity">
                <h3 class="text-2xl text-white font-semibold">{{ selectedCity.name }}</h3>
                <p class="text-white text-xl">Temperature: {{ selectedCity.temperature }}</p>
                <p class="text-white text-xl">Description: {{ selectedCity.description }}</p>
              </div>
              <p v-else class="text-white">Select a city to see the weather details.</p>
        </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'SideBar',
  data() {
    return {
      cityNames: [],         // Array to hold the list of cities
      selectedCity: null,    // Object to hold the selected city's details
      searchText: '',        // Text input for searching cities
      weatherBackground: '', // Variable to hold the background image URL
    };
  },
  computed: {
    filteredCities() {
      // Filter cities based on searchText
      return this.cityNames.filter(city =>
        city.name.toLowerCase().includes(this.searchText.toLowerCase())
      );
    }
  },
  mounted() {
    this.fetchWeatherData();
  },
  methods: {
    async fetchWeatherData() {
      try {
        const response = await axios.get('http://localhost:3000/weather-cities');
        this.cityNames = response.data || [];
      } catch (error) {
        console.error('Error fetching weather data:', error);
        this.cityNames = [];
      }
    },
    async selectCity(city) {
      this.selectedCity = city;
      try {

//         🔑 Keys
// Application ID
// 650577
// Access Key
// Rfm5KuxHNI8dcfLgR8gSdAA8CF1a0YvO0ppFbx4FzqI
// Secret key
// Miq0V_d3z9ze8QPapl5r1jZHWNY0v5higtcuXEKG4R0
// Note: both your Access Key and Secret Key must remain confidential.
        const unsplashAccessKey = 'Rfm5KuxHNI8dcfLgR8gSdAA8CF1a0YvO0ppFbx4FzqI'; // Replace with your Unsplash API access key
        const response = await axios.get(`https://api.unsplash.com/search/photos?page=${Math.floor(Math.random() * 50) + 1}&query=${city.name}&client_id=${unsplashAccessKey}`);
        this.weatherBackground = response.data.results.length > 0 ? response.data.results[0].urls.regular : '';
      } catch (error) {
        console.error('Error fetching city image:', error);
        this.weatherBackground = '';
      }
    },
  },
};
</script>

<style scoped>
.sidebar {

}
</style>
