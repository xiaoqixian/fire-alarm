<template>
  <div>
    <video-grid id="videos"/>
    <WarningTable id="warnings"/>
  </div>
</template>

<script>
import VideoGrid from "./VideosGrid.vue";
import WarningTable from "./Warnings.vue";
export default {
  components: {
    VideoGrid,
    WarningTable
  }
}
</script>

<style>
#videos,
#warnings {
  flex: 1;
  display: flex;
  flex-direction: column;
  border: 1px dashed white;
  padding: 10px;
  margin-bottom: 10px;
}
</style>
