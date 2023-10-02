<template>
  <div id="main">
    <side-window id="side-window" />
    <main-panel/>
  </div>
</template>

<script>
import SideWindow from './SideWindow.vue';
import MainPanel from './MainPanel.vue';

export default {
  name: 'MainWindow',
  components: {
    SideWindow,
    MainPanel
  }
}
</script>

<style scoped>
#main {
  display: flex;
  flex: auto;
  margin-top: 20px;
}
#side-window {
  flex: none;
  height: 100%;
  width: 24%;
  justify-content: flex-start;
  display: flex;
  flex-direction: column;
}

.main-panel {
  flex: none;
  display: flex;
  width: 74%;
  flex-direction: column;
  border: 1px dashed white;
  padding: 5px;
  margin-left: 10px;
}
</style>
