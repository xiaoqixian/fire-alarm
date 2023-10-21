<template>
  <div class="nav-bar">
    <div class="top-bar">
      <div class="all-systems" v-show="toggled">全部系统</div>
      <div class="top-tabs" v-show="!toggled" ref="container">
        <div 
          v-for="(name, key) in tabs" 
          :key="key" 
          class="top-tab" 
          :id="key" 
          ref="topTabs"
          @click="tabSelected" 
          @mouseover="topTabHoverOver" 
          @mouseleave="hoverLeave" 
          :style="currTab == key ? activeTopTabStyle : inactiveTopTabStyle"
          >
          {{name}}
        </div>
      </div>
      <div class="toggle-button" @click="toggleAction">
        <div class="toggle-down" v-show="!toggled">
          <div class="toggle-text">全部</div>
          <div class="toggle-icon"><img src="../assets/toggle_down.png"/></div>
        </div>
        <div class="toggle-up" v-show="toggled">
          <img src="../assets/toggle_up.png"/>
        </div>
      </div>
    </div>
    <div class="full-table" ref="fullTable">
      <div 
        v-for="(name, key) in tabs" 
        :key="key" 
        class="full-tab" 
        :id="key" 
        @click="tabSelected" 
        @mouseover="fullTabHoverOver"
        @mouseleave="hoverLeave"
        :style="currTab == key ? {backgroundColor: 'rgb(71,105,248)'} : {backgroundColor: 'transparent', cursor: 'pointer'}"
      >{{name}}</div>
    </div>
  </div>
</template>

<script>
import {ref} from 'vue';

export default {
  setup: function() {
    return {
      tabsTooLong: ref(false),
      toggled: ref(false),
      height: 0,
      currTab: ref(null),
      hover: ref(true),
    };
  },
  props: {
    tabs: {
      type: Object,
      required: true
    },
    initTab: {
      type: String,
      required: true
    },
    activeTopTabStyle: {
      type: Object,
      default: function() {
        return {
          backgroundColor: "rgb(0, 4, 22)",
          borderTop: "1px solid rgb(71, 105, 248)"
        }
      }
    },
    inactiveTopTabStyle: {
      type: Object,
      default: function() {
        return {
          backgroundColor: "transparent",
          cursor: "pointer"
        }
      }
    }
  },
  mounted: function() {
    this.tabsTooLong = this.$props.tabs.length * 100 > this.$refs.container.offsetWidth;
    this.height = this.$refs.fullTable.offsetHeight;
    this.$refs.fullTable.style.height = this.toggled ? `${this.height}px` : "0px";
    this.currTab = this.$props.initTab;
  },
  emits: ["TabSelected"],
  methods: {
    // eslint-disable-next-line no-unused-vars
    toggleAction: function(_) {
      this.$refs.fullTable.style.height = !this.toggled ? `${this.height}px` : "0px";
      setTimeout(() => this.toggled = !this.toggled, 300);
    },
    tabSelected: function(event) {
      this.$emit("TabSelected", event.target.id);
      /*const newTabObject = this.$refs.topTabs.filter((t) => t.id == event.target.id)[0];*/
      /*//apply activeTopTabStyle to this new tab object*/
      /*Object.entries(this.$props.activeTopTabStyle).forEach(([v, k]) => newTabObject.style[k] = v);*/

      this.currTab = event.target.id;
    },
    topTabColor: function(tt, ct) {
      return tt == ct ? "rgb(0, 4, 22)" : "transparent";
    },
    fullTabColor: function(tt, ct) {
      return tt == ct ? "rgb(71,105,248)" : "transparent";
    },
    topTabHoverOver: function(event) {
      if (this.currTab != event.target.id) {
        event.target.style.backgroundColor = "rgba(255,255,255,0.1)";
      }
    },
    hoverLeave: function(event) {
      if (this.currTab != event.target.id) {
        event.target.style.backgroundColor = "transparent";
      }
    },
    fullTabHoverOver: function(event) {
      if (this.currTab != event.target.id) {
        event.target.style.backgroundColor = "rgb(255,255,255,0.4)";
      }
    },
  }
}
</script>

<style scoped>
.nav-bar {
  position: relative;
}
.top-bar {
  display: flex;
  max-width: 100%;
  height: 100%;
  background-color: inherit;
  color: inherit;
  font-size: 80%;
}
.all-systems {
  flex: auto;
  text-align: left;
  padding-left: 20px;
  background-color: gray;
  height: 100%;
  display: flex;
  align-items: center;
}
.top-tabs {
  flex: auto;
  height: 100%;
  display: flex;
  justify-content: flex-start;
  overflow: hidden;
  background-color: gray;
}
.top-tab {
  margin: none;
  border: none;
  flex: 1;
  min-width: 300px;
  text-align: center;
  white-space: nowrap;
  color: inherit;
  display: flex;
  align-items: center;
  justify-content: center;
}
.toggle-button {
  flex: none;
  width: 100px;
  justify-content: flex-end;
}
.toggle-up,
.toggle-down {
  height: 100%;
  width: 100%;
}
.toggle-down {
  display: flex;
}
.toggle-text {
  text-align: right;
  margin: auto;
  top: 0;
  bottom: 0;
  flex: 1;
  font-size: 70%;
}
.toggle-icon {
  flex: 1;
  position: relative;
  margin-left: 5px;
}
.toggle-up {
  position: relative;
}
.toggle-up img,
.toggle-icon img {
  position: absolute;
  margin: auto;
  top: 0;
  bottom: 0;
  width: 20px;
  height: 20px;
}
.toggle-up img {
  left: 0; 
  right: 0;
}

.full-table {
  width: 100%;
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  grid-auto-rows: 80px;
  transition: height .3s linear;
  flex-wrap: wrap;
  overflow: hidden;
  position: absolute;
  z-index: 9;
  background-color: rgb(0, 4, 22);
}
.full-tab {
  height: 100%;
  width: 100%;
  text-align: center;
  background-color: gray;
  color: white;
  white-space: nowrap;
  font-size: 80%;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
