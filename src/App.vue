<template>
  <Navbar />
  <router-view />
  <Footer />
</template>

<script>
import Footer from './components/Footer.vue';
import Navbar from './components/Navbar.vue';
export default {
  components: {
    Navbar,
    Footer,
  },
};
</script>

<style>
.thing {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
</style>
