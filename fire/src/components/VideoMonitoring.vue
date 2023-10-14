<template>
  <div class="video-monitoring">
    <div class="menu">
      <div 
        class="camera-unit"
        v-for="(unit, i) in camera_info"
        :key="i"
        :id="'cameraUnit' + i"
      >
        <div class="unit-tab">{{unit.name}}</div>
        <div
          class="camera-type"
          v-for="(type, j) in unit.value"
          :key="j"
        >
          <div class="type-tab"><span>{{type.name}}</span></div>

          <div class="cameras" :id="'cameras' + i + j">
            <div
              class="camera"
              v-for="(camera, k) in type.value"
              :key="k"
              :id="'camera' + i + j + k"
            >
              <span>{{camera.name}}</span>
            </div>
          </div>

        </div>
      </div>
    </div>
    <div class="video-display"></div>
  </div>
</template>

<script>
const camera_info = Array.from({length: 5}, 
  (_) => {
    return {
      name: "某某某某单位",
      value: Array.from({length: 5}, 
        (_) => {
          return {
            name: "摄像头类型名称",
            value: Array.from({length: 10}, 
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

export default {
  setup: function() {
    return {
      camera_info
    }
  },
  mounted: function() {
    for (var i = 0; i < this.camera_info.length; i++) {
      for (var j = 0; j < this.camera_info[0].value.length; j++) {
        const el = document.getElementById("cameras" + i + j);
        el.style.height = 0;
        for (const cm of el.children) {
          cm.style.height = 0;
        }
      }
    }
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
.camera-unit .unit-tab {
  background-color: gray;
  height: 30px;
  font-size: 70%;
}

.camera-type .type-tab {
  background: transparent;
  font-size: 70%;
  overflow: hidden;
}
.camera-type .type-tab span {
  padding-left: 10px;
}

.camera {
  font-size: 60%;
  overflow: hidden;
}
.camera span {
  overflow: hidden;
  padding-left: 20px;
  text-overflow: ellipsis;
}

.video-display {
  flex: 3;
}


</style>
