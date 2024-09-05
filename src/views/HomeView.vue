<template>
<!-- 
<nav class="bg-gray-800 p-4">
  <div class="container mx-auto flex justify-between items-center">
    <div class="text-white text-lg font-bold">
      <router-link to="/" class="hover:text-gray-400 transition duration-300 ease-in-out">Home</router-link>
    </div>
    <div class="text-white text-lg font-bold">
      <router-link to="/about" class="hover:text-gray-400 transition duration-300 ease-in-out">About</router-link>
    </div>
  </div>
</nav> -->

  <div class="home">
    <SideBar></SideBar>
  </div>
</template>

<script>
// @ is an alias to /src
import SideBar from '@/components/SideBar.vue'
export default {
  name: 'HomeView',
  components: {
    
    SideBar
  }
}
</script>
