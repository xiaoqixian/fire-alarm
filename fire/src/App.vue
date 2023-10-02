<template>
  <top-banner/>
  <main-window/>
</template>

<script>
import TopBanner from './components/TopBanner.vue';
import MainWindow from './components/Main.vue';

export default {
  name: 'App',
  components: {
    TopBanner,
    MainWindow
  }
}
</script>

<style>
body {
  background-color: rgb(0, 4, 22);
  color: white;
  height: 95vh;
  width: 100%;
  display: flex;
  flex-direction: column;
}
</style>
