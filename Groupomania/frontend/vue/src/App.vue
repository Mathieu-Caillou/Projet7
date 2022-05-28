<template>
<div id="app">
  <Header /><router-view />
  <Footer />
</div>
</template>

<script>
import Footer from './components/Footer.vue';
import Header from './components/Header.vue';

export default {
  name: 'App',
  components: {
    Footer,
    Header
 },
};
</script>

<style>
html, body {
  margin:0;
  padding:0;
  height:100%;
}
</style>
