<template>
  <div class="main-panel">
    <div id="panel-banner">
      <title-banner title="消防系统"/>
      <div id="device-pic">
        <img src="../assets/facility.png"/>
        <div class="title">设施部件图</div>
      </div>
    </div>

    <nav-bar class="nav-bar" :tabs="systemTabs" :initTab="currSystem" @TabSelected="(newSystem) => currSystem=newSystem"></nav-bar>

    <component :is="systems[currSystem]" class="system-panel" v-bind="systemPanelProps"></component>
  </div>
</template>

<script>
import {ref} from 'vue';
import TitleBanner from './TitleBanner.vue';
import NavBar from './NavBar.vue';

import AutoFireAlarm from './AutoFireAlarm.vue';
import ElecFireSystem from './ElecFireSystem.vue';
import IndoorHyrant from './IndoorHydrant.vue';
import FlammableGasMonitoring from './FlammableGasMonitoring.vue';
import EmergencyLightsEvacuation from './EmergencyLightsEvacuation.vue';
import EmergencyBroadcast from './EmergencyBroadcast.vue';
import VideoMonitoring from './VideoMonitoring.vue';

const systems = {
  AutoFireAlarm,
  ElecFireSystem,
  IndoorHyrant,
  FlammableGasMonitoring,
  EmergencyLightsEvacuation,
  EmergencyBroadcast,
  VideoMonitoring
};
const systemTabs = {
  "AutoFireAlarm": "火灾自动报警系统",
  "ElecFireSystem": "电气火灾系统",
  "IndoorHyrant": "室内消火栓系统",
  "FlammableGasMonitoring": "消防应急照明和疏散指示",
  "EmergencyLightsEvacuation": "可燃气体监控系统",
  "EmergencyBroadcast": "应急广播系统",
  "VideoMonitoring": "视频监控系统"
};

const systemPanelProps = {
  flex: "auto"
};

export default {
  setup: function() {
    return {
      systems,
      systemTabs,
      currSystem: ref("ElecFireSystem"),
      systemPanelProps
    }
  },
  components: {
    TitleBanner,
    NavBar,
    AutoFireAlarm,
    ElecFireSystem,
    IndoorHyrant,
    FlammableGasMonitoring,
    EmergencyLightsEvacuation,
    EmergencyBroadcast,
    VideoMonitoring
  },
  methods: {
    systemEntrySelected: function(event) {
      this.systemEntryIndex = event.target.id;
    }
  },
  watch: {
    currSystem: function(newVal) {
      console.log("currSystem changed to " + newVal);
    }
  }
}
</script>

<style scoped>
.main-panel {
  flex: auto;
  display: flex;
}
#panel-banner {
  flex: none;
  display: flex;
  height: 20px;
  margin-bottom: 20px;
}
.title-banner {
  flex: auto;
}
#device-pic {
  display: flex;
  height: 100%;
  flex: none;
  margin-bottom: 10px;
  padding-bottom: 4px;
  border-bottom: 1px solid white;
  justify-content: flex-end;
}
#device-pic img {
  height: 100%;
  width: 20px;
  flex: initial;
  justify-content: flex-start;
  object-fit: contain;
  margin-right: 10px;
}
#device-pic .title {
  height: 100%;
  flex: none;
  font-size: 80%;
  line-height: 100%;
}

.nav-bar {
  width: 100%;
  height: 60px;
  margin-top: 10px;
  flex: none;
}

.system-panel {
  flex: auto;
}
</style>
