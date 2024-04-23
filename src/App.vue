<script setup>
import { RouterLink, RouterView } from 'vue-router'
import Movies from './components/Movies.vue'
import Footer from './components/Footer.vue'
import TopNavbar from './components/TopNavbar.vue'
</script>

<template>
  <link href='https://unpkg.com/boxicons@2.1.4/css/boxicons.min.css' rel='stylesheet'>
  <TopNavbar/>
  <body>
    <Movies/>
  </body>
  <footer>
    <Footer/>
  </footer>

</template>
