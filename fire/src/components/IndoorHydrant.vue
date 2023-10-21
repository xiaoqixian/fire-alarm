<template>
  <div class="indoor-hydrant-panel">
    <title-panel v-bind="titlePanelProps" />
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
          <input type="radio" v-model="currFilter" :value="key" :id="'filter-'+key"/>
          <label :for="'filter-'+key">{{f}}</label>
        </div>
      </div>

      <div class="system-data">
        <div 
          class="cell"
          v-for="(dataEntry, index) in dataList"
          :key="index"
          :class="[dataEntry.status + '-device', dataEntry.network + '-device']"
        >
          <div class="banner">
            <div class="title">
              <div class="serial-number">{{dataEntry.serialNumber}}</div>
              <div class="device-name">{{dataEntry.deviceName}}</div>
            </div>
            <div class="empty-flex-div"></div>
            <div class="status-icon"></div>
          </div>

          <div class="wrapper">
            <div 
              class="chart" 
              :class="{'liquid-ball': dataEntry.type == 'waterLevel'}"
              :id="'chart' + index"></div>
            <table v-if="dataEntry.type == 'waterLevel'">
              <tr v-for="(level, index) in waterLevelList"
                  :key="index"
                :style="{color: level == Math.floor(dataEntry.value) ? 'rgb(255,64,86)' : 'white'}"
                ><td>{{level}}m</td></tr>
            </table>
            <div class="water-level-bar" 
              v-if="dataEntry.type == 'waterLevel'"
              :style="{borderColor: dataEntry.color}">
              <div class="text" :style="{color: dataEntry.color}"><div>{{dataEntry.value}}</div></div>
              <div class="bar"
                   :style="{background: `linear-gradient(${dataEntry.color}, rgb(0,4,22))`, height: `${Math.floor(dataEntry.value/maxWaterLevel*100)}%`}"
                ></div>
            </div>
          </div>
          <div class="device-location">设备位置有点长有点长有点长有点长</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TitlePanel from './TitlePanel.vue';
import "echarts-liquidfill";

const titlePanelProps = {
  title: "室内消火栓系统",
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
  lowPressure: "低压",
  lowWaterLevel: "低水位",
  offline: "离线"
};

let dataList = [
  {
    type: "waterPressure",
    value: 0.25,
    status: "normal",
    network: "online",
    serialNumber: "12312512412",
    deviceName: "智能水压仪(三网通)GGYCSDADKLADJ",
    deviceLocation: "设备位置有点长有点长有点长"
  },
  {
    type: "waterPressure",
    value: 0.35,
    status: "warning",
    network: "online",
    serialNumber: "12312512412",
    deviceName: "智能水压仪(三网通)GGYCSDADKLADJ",
    deviceLocation: "设备位置有点长有点长有点长"
  },
  {
    type: "waterPressureCurve",
    value: Array.from({length: 10}, (_, index) => [index, Math.random()]),
    status: "normal",
    network: "online",
    serialNumber: "12312512412",
    deviceName: "智能水压表(三网通)GGYCSDADKLADJ",
    deviceLocation: "设备位置有点长有点长有点长"
  },
  {
    type: "waterPressure",
    value: 0.00,
    status: "normal",
    network: "offline",
    serialNumber: "12312512412",
    deviceName: "智能水压仪(三网通)GGYCSDADKLADJ",
    deviceLocation: "设备位置有点长有点长有点长"
  },
  {
    type: "waterLevel",
    value: 3.2,
    status: "normal",
    network: "online",
    serialNumber: "12312512412",
    deviceName: "智能水位仪(三网通)GGYCSDADKLADJ",
    deviceLocation: "设备位置有点长有点长有点长"
  },
  {
    type: "waterLevel",
    value: 4.3,
    status: "warning",
    network: "online",
    serialNumber: "12312512412",
    deviceName: "智能水位仪(三网通)GGYCSDADKLADJ",
    deviceLocation: "设备位置有点长有点长有点长"
  },
  {
    type: "waterLevelTempCurve",
    value: {
      level: [[0, 0.5], [5,3], [8,4], [10, 3], [15,2], [25,1.5]],
      temp: [[0, 20], [5, 10], [10, 15], [25,40]]
    },
    status: "normal",
    network: "online",
    serialNumber: "12312512412",
    deviceName: "智能水压仪(三网通)GGYCSDADKLADJ",
    deviceLocation: "设备位置有点长有点长有点长"
  },
  {
    type: "waterLevel",
    value: 0,
    status: "normal",
    network: "offline",
    serialNumber: "12312512412",
    deviceName: "智能水压仪(三网通)GGYCSDADKLADJ",
    deviceLocation: "设备位置有点长有点长有点长"
  },
];
dataList.forEach((entry) => entry.color = entry.status == "warning" ? "rgb(255, 64,86)" : "rgb(71,105,248)");

const waterLevelList = Array.from({length: 4}, (_, i) => i+1).reverse();

const echarts = require("echarts");
export default {
  setup: function() {
    return {
      titlePanelProps,
      subSystems,
      systemFilters,
      waterLevelList,
      dataList,
      maxWaterLevel: 6
    }
  },
  data: function() {
    return {
      currSystem: "sys1",
      currFilter: "all"
    }
  },
  mounted: function() {
    this.dataList.forEach(this.process);
  },
  methods: {
    process: function(dataEntry, index) {
      switch (dataEntry.type) {
        case "waterLevel": 
          this.drawLiquidBall(document.getElementById(`chart${index}`), dataEntry);
          break;
        case "waterPressure":
          this.drawGauge(document.getElementById(`chart${index}`), dataEntry);
          break;
        case "waterPressureCurve":
          this.drawLine(document.getElementById(`chart${index}`), dataEntry);
          break;
        case "waterLevelTempCurve": 
          this.drawDoubleLine(document.getElementById(`chart${index}`), dataEntry);
          break;
      }
    },
    drawLine: function(el, dataEntry) {
      echarts.init(el).setOption({
        grid: {
          top: 10,
          bottom: 10,
          right: 25 
        },
        xAxis: {
          type: 'value',
          axisTick: false,
          axisLabel: false,
          name: "今天",
          nameGap: 0,
          splitLine: {
            show: false
          }
        },
        yAxis: {
          type: 'value',
          axisTick: false,
          min: 0,
          max: 1,
          axisLabel: {
            color: function(value) {
              return value == 0.8 ? "rgb(208, 126, 80)" : "white";
            }
          },
          splitLine: {
            show: false
          }
        },
        visualMap: {
          type: "piecewise",
          show: false,
          dimension: 1,
          pieces: [
            {
              ge: 0,
              lte: 0.8,
              color: "white"
            },
            {
              gt: 0.8,
              lte: 1,
              color: "rgb(208, 126, 80)"
            }
          ]
        },
        series: [
          {
            data: dataEntry.value,
            type: 'line',
            smooth: true,
            showSymbol: false
          }
        ]
      })
    },

    drawDoubleLine: function(el, dataEntry) {
      echarts.init(el).setOption({
        grid: {
          top: 10,
          bottom: 10
        },
        xAxis: {
          type: 'value',
          axisTick: false,
          axisLabel: false,
          name: "今天",
          nameGap: 0,
          splitLine: {
            show: false
          }
        },
        yAxis: [{
          type: 'value',
          axisTick: false,
          min: 0,
          max: 5,
          splitLine: {
            show: false,
            lineStyle: {
              color: "white"
            }
          },
        },
        {
          type: 'value',
          axisTick: false,
          splitLine: {
            show: false,
            lineStyle: {
              color: "white"
            }
          },
        }
        ],
        series: [
          {
            data: dataEntry.value.level,
            type: 'line',
            smooth: true,
            showSymbol: false,
            yAxisIndex: 0
          },
          {
            data: dataEntry.value.temp,
            type: 'line',
            smooth: true,
            showSymbol: false,
            lineStyle: {
              color: "green"
            },
            yAxisIndex: 1
          }
        ]
      })
    },

    drawLiquidBall: function(el, dataEntry) {
      echarts.init(el).setOption({
        series: [{
          type: 'liquidFill',
          radius: "70%",
          center: ["50%", "50%"],
          data: [{
            value: dataEntry.value/this.maxWaterLevel,
            itemStyle: {
              color: dataEntry.color
            },
          }],
          backgroundStyle: {
            color: "transparent"
          },
          outline: {
            borderDistance: 3,
            itemStyle: {
              borderColor: dataEntry.color,
              borderWidth: 3
            }
          },
          label: {
            fontSize: 20,
            color: "white",
            formatter: function() {
              return `${dataEntry.value}m`
            }
          }
        }]
      });
    },
    drawGauge: function(el, dataEntry) {
      echarts.init(el).setOption({
        series: [
          {
            name: "Pressure",
            type: "gauge",
            radius: "90%",
            progress: {
              show: true,
              width: 5
            },
            axisLine: {
              lineStyle: {
                width: 5
              }
            },
            splitLine: {
              distance: 5
            },
            axisTick: {
               distance: 5
            },
            axisLabel: {
              show: false,
              distance: 8
            },
            detail: {
              valueAnimation: true,
              formatter: "{value}"
            },
            pointer: {
              width: 3,
              itemStyle: {
                color: "rgb(208, 126, 80)"
              }
            },
            min: 0,
            max: 0.5,
            data: [
              {
                value: dataEntry.value,
                name: "MPa",
                itemStyle: {
                  color: "rgb(71,105,248)"
                },
                title: {
                  offsetCenter: [0, "-10%"]
                }
              }
            ]
          }
        ]
      });
    }
  },
  components: {
    TitlePanel
  }
}
</script>

<style scoped>
.indoor-hydrant-panel {
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

.cell {
  margin-bottom: 10px;
}
.cell .banner {
  display: flex;
  height: 25%;
  margin-bottom: 10px;
  margin-right: 10px;
}
.cell .banner .title {
  flex: none;
  margin-left: 5px;
  max-width: 80%;
}
.cell .banner .title .serial-number {
  font-size: 60%;
  color: gray;
}
.cell .banner .title .device-name {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  width: 80%;
  font-size: 70%;
}
.empty-flex-div {
  flex: auto;
}

.cell .banner .status-icon {
  height: 80%;
  aspect-ratio: 1/1;
  flex: none;
  justify-content: flex-end;
  align-items: center;
  background-size: 100% 100%;
  background-repeat: no-repeat;
}
.normal-device .banner .status-icon {
  background-image: url("../assets/status_normal.png");
}
.warning-device .banner .status-icon {
  background-image: url("../assets/status_warning.png");
}
.offline-device .banner .status-icon {
  background-image: url("../assets/status_offline.png");
}
.cell .banner .status-icon img {
  height: 70%;
  object-fit: contain;
}

.cell .wrapper {
  height: 150px;
  display: flex;
}
.cell .chart {
  flex: 3;
}
.cell .chart canvas {
  width: 100%;
}

.cell .wrapper table {
  flex: 1;
  height: 80%;
}
.cell .wrapper table td {
  text-align: center;
}
.cell .wrapper .water-level-bar {
  flex: 1;
  border-style: solid;
  border-width: 2px;
  background-color: transparent;
  margin: 2% 10%;
  display: flex;
  flex-direction: column;
}
.cell .wrapper .water-level-bar .text {
  position: relative;
  flex: auto;
}
.cell .wrapper .water-level-bar .text div {
  width: 100%;
  position: absolute;
  bottom: 5px;
  left: 0; right: 0;
  text-align: center;
  font-size: 70%;
  font-weight: bold;
}
.cell .wrapper .water-level-bar .bar {
  margin: 5px;
  border: none;
}

.cell .device-location {
  width: 80%;
  height: 10%;
  justify-content: flex-end;
  color: gray;
  font-size: 60%;
  text-overflow: hidden;
}
</style>
