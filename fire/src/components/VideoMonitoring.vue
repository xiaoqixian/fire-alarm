<template>
  <div class="video-monitoring">
    <div class="menu">
      <div 
        class="video-group"
        v-for="(gp, i) in camera_info"
        :key="i"
        :id="'video-group' + i"
      >
        <div class="group-title">
          <div class="title">{{gp.name}}</div>
          <img :src="toggleDownImg" @click="toggleGroup(i)"/>
        </div>
        <div class="group-wrapper">
          <div
            class="video-subgroup"
            v-for="(sgp, j) in gp.value"
            :key="j"
            :id="'video-subgroup' + i + j"
          >
            
            <div class="subgroup-title">
              <div class="title">{{sgp.name}}</div>
              <img :src="toggleDownImg" @click="toggleSubgroup(i, j)"/>
            </div>
            <div class="subgroup-wrapper">
              <div
                class="video-tab"
                v-for="(v, k) in sgp.value"
                :id="'camera' + i + j + k"
                :key="k"
                @click="selectCamera(i, j, k)"
              >
                <img :src="cameraIcon"/>
                <div class="title">{{v.name}}</div>
              </div>
            </div>

          </div>
        </div>
      </div>
    </div>

    <div class="video-display">
      <div class="banner">
        <img :src="squareSelectedIcon"/>
        <img :src="winIcon"/>
        <div class="empty-flex-div"></div>
        <search-box v-bind="searchBoxProps" />
        <button type="button">导出</button>
      </div>
      <div class="video-frame"></div>
    </div>
  </div>
</template>

<script>
const camera_info = Array.from({length: 5}, 
  // eslint-disable-next-line no-unused-vars
  (_) => {
    return {
      name: "某某某某单位",
      value: Array.from({length: 5}, 
        // eslint-disable-next-line no-unused-vars
        (_) => {
          return {
            name: "摄像头类型名称",
            value: Array.from({length: 10}, 
              // eslint-disable-next-line no-unused-vars
              (_) => {
                return {
                  name: "某某某号楼某某地摄像头"
                }
              }
            )
          }
        }
      )
    }
  }
);

const searchBoxProps = {
  style: {
    width: "300px",
    height: "70%",
    flex: "none",
    margin: "auto",
    top: 0,
    bottom: 0,
    border: "1px solid white"
  },
  placeholder: "摄像头名称/位置",
  backgroundImage: 'url(' + require("../assets/search.png") + ')'
};

import SearchBox from "./SearchBox.vue";

export default {
  setup: function() {
    return {
      camera_info,
      groupHeights: [],
      subgroupHeights: [],
      toggleDownImg: require("../assets/right_arrow.png"),
      toggleUpImg: require("../assets/toggle_up.png"),
      cameraIcon: require("../assets/camera_white.png"),
      cameraSelectedIcon: require("../assets/camera_blue.png"),
      squareIcon: require("../assets/square_white.png"),
      squareSelectedIcon: require("../assets/square_blue.png"),
      winIcon: require("../assets/win_white.png"),
      winSelectedIcon: require("../assets/win_blue.png"),
      selectedCamera: null,
      searchBoxProps
    }
  },
  mounted: function() {
    document.querySelectorAll(".video-group").forEach((group) => {
      let currSubgroupHeights = [];

      group.querySelectorAll(".video-subgroup").forEach((subgroup) => {
        const subgroupWrapperEl = subgroup.querySelector(".subgroup-wrapper");
        currSubgroupHeights.push(subgroupWrapperEl.offsetHeight);
        subgroupWrapperEl.style.height = "0px";
      });

      this.subgroupHeights.push(currSubgroupHeights);

      const groupWrapperEl = group.querySelector(".group-wrapper");
      this.groupHeights.push(groupWrapperEl.offsetHeight);
      groupWrapperEl.style.height = "0px";
    });
  },

  methods: {
    toggleGroup: function(i) {
      const groupEl = document.querySelector("#video-group" + i);

      const groupWrapperEl = groupEl.querySelector(".group-wrapper");
      groupWrapperEl.style.height = groupWrapperEl.style.height == "0px" ? `${this.groupHeights[i]}px` : "0px";

      const groupImgEl = groupEl.querySelector(".group-title img");
      groupImgEl.src = groupImgEl.src == this.toggleUpImg ? this.toggleDownImg : this.toggleUpImg;
    },

    toggleSubgroup: function(i, j) {
      const subgroupEl = document.querySelector("#video-subgroup" + i + j);
      const subgroupWrapperEl = subgroupEl.querySelector(".subgroup-wrapper");

      if (subgroupWrapperEl.style.height == "0px") {
        subgroupWrapperEl.style.height = `${this.subgroupHeights[i][j]}px`;
        this.groupHeights[i] += this.subgroupHeights[i][j];
      } else {
        subgroupWrapperEl.style.height = "0px";
        this.groupHeights[i] -= this.subgroupHeights[i][j];
      }

      const subgroupImgEl = subgroupEl.querySelector(".subgroup-title img");
      subgroupImgEl.src = subgroupImgEl.src == this.toggleDownImg ? this.toggleUpImg : this.toggleDownImg;

      document.querySelector("#video-group" + i + " .group-wrapper").style.height = `${this.groupHeights[i]}px`;
    },

    selectCamera: function(i, j, k) {
      const newCamera = [i, j, k].join("");
      if (newCamera === this.selectedCamera) {
        return;
      }

      if (this.selectedCamera) {
        const prevCameraEl = document.querySelector("#camera" + this.selectedCamera);
        prevCameraEl.style.borderColor = "transparent";
        prevCameraEl.querySelector("img").src = this.cameraIcon;
      }

      const cameraEl = document.querySelector("#camera" + newCamera);
      cameraEl.style.borderColor = "rgb(71,105,248)";
      cameraEl.querySelector("img").src = this.cameraSelectedIcon;

      this.selectedCamera = newCamera;
    }
  },
  components: {
    SearchBox
  }
}
</script>

<style scoped>
.video-monitoring {
  display: flex;
  height: 100%;
  width: 100%;
  overflow-y: auto;
  overflow-x: hidden;
}

.menu {
  flex: 1;
  color: white;
}

.video-group {
  display: grid;
  grid-template-columns: 1fr;
  grid-auto-rows: auto;
  font-size: 70%;
}

.video-group .group-title {
  background-color: gray;
  display: flex;
  padding-right: 15px;
}
.video-group .group-title .title {
  padding-left: 10px;
  flex: auto;
}
.video-group .group-title img {
  flex: none;
  height: 20px;
  aspect-ratio: 1/1;
  cursor: pointer;
}

.video-group .group-wrapper {
  overflow: hidden;
  transition: height .2s linear;
}
.video-group .video-subgroup {
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: auto;
  overflow: hidden;
}
.video-group .video-subgroup .subgroup-title {
  padding-right: 15px;
  display: flex;
}
.video-subgroup .subgroup-title .title {
  padding-left: 20px;
  flex: auto;
}
.video-subgroup .subgroup-title img {
  flex: none;
  height: 20px;
  aspect-ratio: 1/1;
  cursor: pointer;
}

.video-group .video-subgroup .subgroup-wrapper {
  transition: height .2s linear;
}

.video-tab {
  display: flex;
  padding-left: 30px;
  position: relative;
  border-width: 2px;
  border-style: solid;
  border-color: transparent;
  cursor: pointer;
}
.video-tab img {
  flex: none;
  height: 15px;
  aspect-ratio: 1/1;
  margin: auto;
  top: 0; bottom: 0;
}
.video-tab .title {
  flex: auto;
  padding-left: 10px;
}

.video-display {
  flex: 3;
  display: flex;
  flex-direction: column;
}

.video-display .banner {
  height: 50px;
  flex: none;
  position: relative;
  display: flex;
}
.video-display .banner img {
  height: 90%;
  aspect-ratio: 1/1;
  margin: auto;
  top: 0; bottom: 0;
  flex: none;
}
.video-display .banner .empty-flex-div {
  flex: auto;
}
.video-display .banner button {
  flex: none;
  width: 60px;
  height: 70%;
  background-color: rgb(71,105,248);
  color: white;
  font-size: 60%;
  margin: auto 30px;
  top: 0; bottom: 0;
}

.video-display .video-frame {
  flex: auto;
}

</style>
