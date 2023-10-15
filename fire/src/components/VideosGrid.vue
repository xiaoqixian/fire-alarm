<template>
  <div>
    <title-banner title="视频联动"/>
    <search-box v-bind="searchBoxProps"/>
    <div id="videos-grid">
      <div class="grid-video"></div>
      <div class="grid-video"></div>
      <div class="grid-video"></div>
      <div class="grid-video"></div>
    </div>
  </div>
</template>

<script>
import TitleBanner from './TitleBanner.vue';
import SearchBox from './SearchBox.vue';
export default {
  setup: function() {
    return {
      searchBoxProps: {
        style: {
          width: "100%",
          height: "30px",
          flex: "none",
          marginBottom: "10px"
        },
        placeholder: "请输入名称/位置",
        backgroundImage: 'url(' + require("../assets/search.png") + ')'
      }
    }
  },
  components: {
    TitleBanner,
    SearchBox
  }
}
</script>

<style>
#searchTerm {
  border: none;
  flex: auto;
  height: 100%;
  background-color: inherit;
  font-size: 50%;
}
#searchTerm:focus {
  color: white;
}
#searchButton {
  flex: initial;
  justify-content: flex-end;
  border: none;
  cursor: pointer;
  align-content: center;
  padding: 2px;
}

#videos-grid {
  display: grid;
  gap: 10px;
  grid-template-columns: 1fr 1fr;
  flex: auto;
  overflow: auto;
  
} 
#videos-grid > .grid-video {
  height: 100px;
  background-color: rgba(255, 255, 255, 0.1);
  border: none;
  padding: 0;
}
</style>
