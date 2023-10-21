<template>
  <div class="elec-fire-panel">
    <title-panel v-bind="titlePanelProps"></title-panel>
    <div class="main-panel">
      <div class="system-filters">
        <select v-model="currSystem" class="select-system">
          <option 
            v-for="(name, key) in subSystems" 
            :value="key" 
            :key="key">
          {{name}}
          </option>
        </select>
        <div class="system-choice" v-for="(f, key) in systemFilters" :key="key">
          <input type="radio" v-model="filterSelected" :value="key" :id="'filter-'+key"/>
          <label :for="'filter-'+key">{{f}}</label>
        </div>
      </div>
      <div class="system-data">
        <div
          class="device-cell"
          v-for="(device, dIndex) in displayData"
          :key="dIndex"
          :id="'device-' + dIndex"
          :class="deviceStatusClass(device)"
        >
          <div class="banner">
            <div class="title">
              <div class="serial-number">{{device.serialNumber}}</div>
              <div class="device-location">{{device.location}}</div>
            </div>
            <div class="empty-flex-div"></div>
            <div class="device-status-icon"></div>
          </div>

          <div class="data-panel">
            <div 
              v-for="(cell, cIndex) in device.data"
              :key="cIndex"
              class="data-cell"
              :id="'device-'+dIndex+'-cell-'+cIndex"
              :class="cellStatusClass(cell)"
            >
              <div class="name">{{cell.name}}</div>
              <div class="value">{{cell.value}}</div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="empty-flex-div"></div>
    <page-switching 
      :initialPage="initialPage"
      :totalPage="totalPage"
      :style="pageSwitchingStyle"
      ref="pageSwitchingComp"
      @jumpPage="jumpPage"
    />
  </div>
</template>

<script>
import {ref} from 'vue';
import TitlePanel from './TitlePanel.vue';
import PageSwitching from './PageSwitching.vue';
const titlePanelProps = {
  title: "电气火灾系统",
  description: [
    "系统位置: 某某某某位置系统",
    "报警主机: 某某主机12A",
    "信息传输: 某某传输装置",
    "运行状态: 正常运行"
  ],
  data: {
    "设备总数": 66,
    "设备报警数": 3,
    "离线设备数": 2
  },
  style: {
    width: "100%",
    height: "20%",
    flex: "none"
  }
};

const subSystems = {
  sys1: "某某某某系统一",
  sys2: "某某某某系统二"
};
const systemFilters = {
  all: "全部", 
  normal: "正常", 
  warning: "报警",
  offline: "离线"
};

function getDeviceData() {
  // eslint-disable-next-line no-unused-vars
  return [...new Array(32)].map((i) => {
    let temp = {
      serialNumber: "10034561223456",
      location: "设备位置有点长有点长有点长",
      status: Math.random() > 0.9 ? "offline":"normal",
      // eslint-disable-next-line no-unused-vars
      data: [...new Array(6)].map((j) => {
        return { 
          name:"A相电压", 
          value:"238.2", 
          status: Math.random() > 0.8 ? "warning" : "normal"
        };
      })
    }; 
    if (temp.status == "normal") {
      temp.status = temp.data.some((item) => item.status == "warning") ? "warning" : "normal";
    }
    return temp;
  });
}

const deviceData = getDeviceData();
const normalData = deviceData.filter((d) => d.status == "normal");
const offlineData = deviceData.filter((d) => d.status == "offline");
const warningData = deviceData.filter((d) => d.status == "warning");
const dataAlternatives = {
  all: deviceData,
  normal: normalData,
  offline: offlineData,
  warning: warningData
};
const displayWindowSize = 8;

/*const pageSwitchingProps = ref({*/
  /*initialPage: 1,*/
  /*totalPage: Math.ceil(deviceData.length / displayWindowSize),*/
  /*style: {*/
    /*width: "100%",*/
    /*height: "30px",*/
    /*marginBottom: "10px",*/
    /*marginRight: "10px",*/
    /*justifyContent: "flex-end",*/
    /*flex: "none"*/
  /*}*/
/*});*/


export default {
  setup: function() {
    return {
      titlePanelProps,
      subSystems,
      systemFilters,
      pageSwitchingStyle: {
        width: "100%",
        height: "30px",
        marginBottom: "10px",
        marginRight: "10px",
        justifyContent: "flex-end",
        flex: "none"
      },
      filterSelected: ref("all"),
      currSystem: ref("sys1"),
    }
  },
  data: function() {
    return {
      displayData: dataAlternatives[this.filterSelected].slice(0, displayWindowSize),
      initialPage: 1,
      totalPage: Math.ceil(dataAlternatives["all"].length / displayWindowSize)
    }
  },
  computed: {
    dataSource: () => dataAlternatives[this.filterSelected]
  },
  mounted: function() {
    /*setInterval(() => this.deviceData = getDeviceData(), 3000);*/
  },
  components: {
    TitlePanel,
    PageSwitching
  },
  watch: {
    filterSelected: function(newVal) {
      this.totalPage = Math.ceil(dataAlternatives[newVal].length / displayWindowSize);
      this.displayData = dataAlternatives[newVal].slice(0, displayWindowSize);
    }
  },
  methods: {
    deviceStatusClass: function(device) {
      return device.status == "offline" ? "offline-device" : 
        (device.data.some(item => item.status == "warning") ? 
          "warning" : "normal") + "-device";
    },
    cellStatusClass: function(cell) {
      return cell.status + "-cell";
    },
    jumpPage: function(pageIndex) {
      const start = (pageIndex - 1) * displayWindowSize;
      const end = start + displayWindowSize;
      this.displayData = dataAlternatives[this.filterSelected].slice(start, end)
    }
  }
}
</script>

<style scoped>
.elec-fire-panel {
  display: flex;
  flex-direction: column;
}
.main-panel {
  flex: none;
}
.system-filters {
  width: 100%;
  height: 30px;
  display: flex;
}
.select-system {
  flex: none;
  width: 180px;
  height: 100%;
  background-color: inherit;
  color: inherit;
}
.system-choice {
  flex: none;
  width: 100px;
  height: 100%;
  line-height: 100%;
  font-size: 80%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.system-data {
  flex: auto;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: auto;
  margin-top: 10px;
  gap: 5px;
}

.device-cell .banner {
  display: flex;
  height: 25%;
  margin-bottom: 10px;
  margin-right: 20px;
}
.device-cell .banner .title {
  flex: none;
  margin-left: 5px;
  max-width: 80%;
}

.device-cell .banner .title .serial-number {
  font-size: 60%;
  color: gray;
}
.device-cell .banner .title .device-location {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  width: 80%;
  font-size: 70%;
}
.empty-flex-div {
  flex: auto;
}
.device-cell .banner .device-status-icon {
  height: 80%;
  aspect-ratio: 1/1;
  flex: none;
  justify-content: flex-end;
  align-items: center;
  background-size: 100% 100%;
  background-repeat: no-repeat;
}
.normal-device .banner .device-status-icon {
  background-image: url("../assets/status_normal.png");
}
.warning-device .banner .device-status-icon {
  background-image: url("../assets/status_warning.png");
}
.offline-device .banner .device-status-icon {
  background-image: url("../assets/status_offline.png");
}
.device-cell .banner .device-status-icon img {
  height: 70%;
  object-fit: contain;
}
.device-cell .data-panel {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: 40px;
  gap: 5px;
  margin: 5px;
}

.device-cell .data-panel .data-cell {
  background-size: 100% 50%;
  background-repeat: no-repeat;
}
.device-cell .data-panel .normal-cell {
  background-image: url("../assets/cell_normal.png");
}
.warning-device .data-panel .warning-cell {
  background-image: url("../assets/cell_warning.png");
}
.device-cell .data-panel .data-cell .name {
  font-size: 50%;
  color: gray;
  position: relative;
  left: 5px;
  margin-bottom: 3px;
}
.device-cell .data-panel .data-cell .value {
  font-size: 80%;
  position: relative;
  left: 5px;
}
.warning-device .warning-cell .value {
  color: red;
}
.normal-device .normal-cell .value,
.warning-device .normal-cell .value {
  color: white;
}
.offline-device .data-cell .value {
  color: gray;
}
</style>
