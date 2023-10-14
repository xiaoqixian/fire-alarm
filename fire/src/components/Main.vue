<template>
  <div class="main-window">
    <div class="side-panel">
      <component :is="TopLeftComp" class="top-left-comp"/>
      <component :is="BottomLeftComp" class="bottom-left-comp"/>
    </div>
    <main-panel 
      class="main-panel"
      :initTab="initTab"
      @TabSelected="(newTab) => currTab = newTab"
    />
  </div>
</template>

<script>
import VideoGrid from "./VideosGrid.vue";
import WarningTable from "./Warnings.vue";
import PowerConsumption from "./PowerConsumptionManagement.vue";
import MainPanel from './MainPanel.vue';

//registry of side windows for each tab
function tabSideWindowRegistry(tab) {
  switch (tab) {
    case "AutoFireAlarm":
      return ["power-consumption", "warning-table"]
    default: 
      return ["video-grid", "warning-table"]
  }
};

export default {
  name: 'MainWindow',
  setup: function() {
    return {
      initTab: "VideoMonitoring"
    }
  },
  data: function() {
    const [TopLeftComp, BottomLeftComp] = tabSideWindowRegistry(this.initTab);
    return {
      TopLeftComp,
      BottomLeftComp,
      currTab: this.initTab
    }
  },
  watch: {
    currTab: function(newTab) {
      [this.TopLeftComp, this.BottomLeftComp] = tabSideWindowRegistry(newTab);
    }
  },
  components: {
    VideoGrid,
    WarningTable,
    PowerConsumption,
    MainPanel,
  }
}
</script>

<style scoped>
.main-window {
  flex: auto;
  width: 100%;
  margin-top: 20px;
  display: flex;
}
.side-panel {
  height: 100%;
  flex: 1;
}
.side-panel .top-left-comp,
.side-panel .bottom-left-comp {
  width: 100%;
  height: 50%;
}
.main-panel {
  flex: 3;
  display: flex;
  width: 74%;
  height: 100%;
  flex-direction: column;
  padding: 5px;
  margin-left: 10px;
}
</style>
