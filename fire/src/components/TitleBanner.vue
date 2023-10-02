<template>
  <div class="title-banner">
    <img src="../assets/title_icon.png"/>
    <div class="title">{{title}}</div>
  </div>
</template>

<script>
export default {
  props: {
    title: String
  }
}
</script>

<style>
.title-banner {
  display: flex;
  height: 20px;
  width: 100%;
  flex: none;
  margin-bottom: 10px;
  padding-bottom: 4px;
  border-bottom: 1px solid white;
}
.title-banner img {
  flex: none;
  aspect-ratio: 1/1;
  justify-content: flex-start;
  object-fit: contain;
  margin-right: 10px;
}
.title-banner .title {
  height: 100%;
  flex: auto;
  font-size: 80%;
  line-height: 100%;
}
</style>
