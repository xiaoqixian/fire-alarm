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
  width: 95vw;
  display: flex;
  font-family: Arial, Helvetica, sans-serif;
}
#app {
  justify-content: center;
  height: 100%;
  width: 100%;
  display: flex;
  flex-direction: column;
}

.top-banner {
  height: 10%;
}

.main-window {
  height: 88%;
}
</style>
