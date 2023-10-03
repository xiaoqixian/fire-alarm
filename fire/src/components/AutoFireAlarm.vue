<template>
  <div class="auto-fire-panel">
      <select v-model="currSystem" class="system-selection">
        <option 
          v-for="(name, key) in subSystems" 
          :value="key" 
          :key="key">
        {{name}}
        </option>
      </select>
    <title-panel v-bind="titlePanelProps"/>
  </div>
</template>

<script>
import TitlePanel from './TitlePanel.vue';
import {ref} from 'vue';

const subSystems = {
  sys1: "某某某某系统一",
  sys2: "某某某某系统二"
};
const titlePanelProps = {
  title: "火灾自动报警系统",
  description: [
    "系统位置: 某某某某位置系统",
    "报警主机: 某某主机12A",
    "信息传输: 某某传输装置",
    "运行状态: 正常运行"
  ],
  data: {
    "部件总数": 180,
    "报警部件数": 3,
    "离线部件数": 2
  },
  style: {
    width: "100%",
    height: "20%",
    flex: "none",
  }
};

export default {
  setup: function() {
    return {
      currSystem: ref("sys1"),
      subSystems,
      titlePanelProps
    }
  },
  components: {
    TitlePanel
  }
}
</script>

<style scoped>
.auto-fire-panel {
  display: flex;
  flex-direction: column;
}

.system-selection {
  width: 180px;
  height: 30px;
  background-color: inherit;
  color: inherit;
}
</style>
