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

    <div class="data-table"
      v-on-click-outside="() => selectedRowIndex = null">
      <div class="trow thead">
        <div 
          class="th non-filter"
          v-for="(item, index) in ['部件类型', '设备ID', '所属建筑', '楼层(区域)']"
          :key="index"
        >
        {{item}}
        </div>

        <div class="th filter"
          v-on-click-outside="() => networkToggle = false">
          <div class="button">
            <div class="label">联网状态</div>
            <div class="icon" @click="networkToggle = !networkToggle">
              <img src="../assets/toggle_down.png"/>
            </div>
          </div>
          <div class="fold-menu" ref="networkFoldMenu">
            <div
              class="fold-entry"
              v-for="(val, key, index) in foldMenuEntries['network']"
              :key="index"
              @click="currEntries.network = key"
              :style="key == currEntries.network ? selectedFoldEntryStyle : nonSelectedFoldEntryStyle"
            >{{val}}</div>
          </div>
        </div>

        <div class="th filter"
          v-on-click-outside="() => statusToggle = false">
          <div class="button">
            <div class="label">设备状态</div>
            <div class="icon" @click="statusToggle = !statusToggle">
              <img src="../assets/toggle_down.png"/>
            </div>
          </div>
          <div class="fold-menu" ref="statusFoldMenu">
            <div
              class="fold-entry"
              v-for="(val, key, index) in foldMenuEntries['status']"
              :key="index"
              @click="currEntries.status = key"
              :style="key == currEntries.status ? selectedFoldEntryStyle : nonSelectedFoldEntryStyle"
            >{{val}}</div>
          </div>
        </div>
      </div>

      <div 
        class="trow tbody"
        v-for="(row, row_index) in displayWindow"
        :key="row_index"
        :class="row_index == selectedRowIndex ? 'tbody-row-selected' : 'tbody-row-notselected'"
        @click="selectedRowIndex = row_index"
      >
        <div
          class="td"
          v-for="(val, key, col_index) in row"
          :key="col_index"
          :class="col_index > 3 ? 'filter' : 'non-filter'"
        >
        <span 
          v-if="['normal', 'warning', 'online', 'offline'].includes(val)"
          :style="dotStyle(val)">•</span>
        {{textMapping(val)}}
        </div>
      </div>
    </div>

    <div class="empty-flex-div"></div>

    <page-switching 
      initialPage=1
      :totalPage="totalPage"
      @jumpPage="jumpPage"
      :style="pageSwitchingStyle"
    />
  </div>
</template>

<script>
import {reactive} from 'vue';
import TitlePanel from './TitlePanel.vue';
import PageSwitching from './PageSwitching.vue';
import OnClickOutside from './OnClickOutside.vue';

function getData(row) {
  return [...new Array(row)].map((r) => {
    return {
      unitType: "部件类型部件类型",
      serialNumber: "123571893412312",
      location: "某某某某大厦某某某大楼",
      partition: "F2(某某某某分区)",
      network: Math.random() > 0.95 ? "offline" : "online",
      status: Math.random() > 0.7 ? "warning" : "normal"
    };
  });
}

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
const selectedFoldEntryStyle = {
  backgroundColor: "rgba(71,105,248,0.7)",
  cursor: "not-allowed"
};
const nonSelectedFoldEntryStyle = {
  backgroundColor: "#48494b",
  cursor: "pointer"
};

const displayWindowSize = 8;
const sourceData = getData(50);

export default {
  name: "AutoFireAlarm",
  setup: function() {
    return {
      titlePanelProps,
      subSystems,
      selectedFoldEntryStyle,
      nonSelectedFoldEntryStyle,
      currEntries: reactive({
        network: "all",
        status: "all"
      }),
      foldButtonLabels: {
        network: "联网状态",
        status: "设备状态"
      },
      foldMenuEntries: {
        network: {
          all: "全部",
          online: "在线",
          offline: "离线"
        },
        status: {
          all: "全部",
          normal: "正常",
          warning: "报警"
        }
      },
      foldMenuHeights: {
        networkFoldMenu: 0,
        statusFoldMenu: 0
      },
      pageSwitchingStyle: {
        width: "100%",
        height: "30px",
        marginBottom: "10px",
        marginRight: "10px",
        justifyContent: "flex-end",
        flex: "none"
      }
    }
  },
  data: function() {
    return {
      currSystem: "sys1",
      networkToggle: false,
      statusToggle: false,
      displaySource: null,
      displayWindow: null, //filled in watch currEntries handler
      totalPage: null,
      selectedRowIndex: null
    }
  },
  watch: {
    networkToggle: function(newVal) {
      this.menuTransition("networkFoldMenu", newVal);
    },
    statusToggle: function(newVal) {
      this.menuTransition("statusFoldMenu", newVal);
    },
    currEntries: {
      handler: function(newVal) {
        this.displaySource = sourceData.filter((item) => {
          return (newVal.network == "all" || newVal.network == item.network) &&
          (newVal.status == "all" || newVal.status == item.status)
        });
        this.displayWindow = this.displaySource.slice(0, displayWindowSize);
      },
      deep: true,
      immediate: true
    },
    displaySource: {
      handler: function(newVal) {
        this.totalPage = Math.ceil(newVal.length / displayWindowSize);
      },
      immediate: true
    }
  },
  mounted: function() {
    Object.keys(this.foldMenuHeights).forEach((key) => {
      this.foldMenuHeights[key] = this.$refs[key].offsetHeight;
    });
    if (!this.networkToggle) {
      this.$refs.networkFoldMenu.style.height = "0px";
    }
    if (!this.statusToggle) {
      this.$refs.statusFoldMenu.style.height = "0px";
    }
  },
  methods: {
    menuTransition: function(container, toggle) {
      this.$refs[container].style.height = toggle ? `${this.foldMenuHeights[container]}px` : "0px";
    },
    textMapping: function(text) {
      switch(text) {
        case "normal":
          return "正常";
        case "warning":
          return "报警";
        case "online":
          return "在线";
        case "offline":
          return "离线";
        default:
          return text;
      }
    },
    jumpPage: function(newPage) {
      const start = (newPage - 1) * displayWindowSize;
      const end = start + displayWindowSize;
      this.displayWindow = this.displaySource.slice(start, end);
    },
    dotStyle: function(item) {
      return {
        color: ["normal", "online"].includes(item) ? "rgb(0,128,0)" : "rgb(255,0,0)",
        fontSize: "20px",
        height: "100%",
        verticalAlign: "middle"
      }
    }
  },
  components: {
    TitlePanel,
    PageSwitching
  },
  directives: {
    OnClickOutside
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

.data-table {
  display: flex;
  flex-direction: column;
  width: 100%;
  flex: none;
  font-size: 70%;
}
.data-table .trow {
  display: flex;
  height: 30px;
}
.data-table .trow .non-filter {
  flex: 2;
  margin: auto;
  top: 0; bottom: 0;
  text-align: center;
}
.data-table .trow .filter {
  flex: 1;
  position: relative;
}

.data-table .thead .filter .button {
  height: 100%;
  display: flex;
}
.data-table .thead .filter .button .label {
  flex: 3;
  margin: auto;
  top: 0; bottom: 0;
  margin-right: 5px;
  text-align: end;
}
.data-table .thead .filter .button .icon {
  flex: 1;
  height: 100%;
  cursor: pointer;
}
.data-table .thead .filter .button .icon img {
  height: 40%;
  position: relative;
  top: 20%;
  aspect-ratio: 1/1;
  object-fit: contain;
}
.data-table .thead .filter .fold-menu {
  width: 80%;
  overflow: hidden;
  text-align: center;
  transition: height .3s linear;
  position: absolute;
  margin: auto; left: 0; right: 0;
  z-index: 9;
}
.data-table .thead .filter .fold-menu .fold-entry {
  margin: none;
  border: 1px solid white;
  height: 20px;
  font-size: 15px;
  cursor: pointer;
}

.data-table .tbody .td {
  margin: auto;
  top: 0; bottom: 0;
  text-align: center;
}
.data-table .tbody-row-notselected:hover {
  background-color: rgba(255,255,255,0.1);
  cursor: pointer;
}
.data-table .tbody-row-selected {
  border: 1px solid rgb(71,105,248);
}

.empty-flex-div {
  flex: auto;
}
</style>
