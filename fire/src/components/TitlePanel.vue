<template>
  <div class="title-panel">
    <div class="title">{{title}}</div>
    <div class="description">
      <div class="desc-line" v-for="l in description" :key="l">{{l}}</div>
    </div>
    <div class="data" v-for="(value, key) in data" :key="key">
      <div class="key">{{key}}</div>
      <div class="value">{{value}}</div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    title: String,
    description: Array,
    data: Object
  }
}
</script>

<style scoped>
.title-panel {
  display: flex;
}
.title {
  flex: 1.5;
  position: relative;
  margin: auto;
  top: 0; 
  bottom: 0;
  padding-left: 30px;
  font-size: 150%;
  white-space: nowrap;
  color: rgb(71,105,248);
}
.description {
  flex: 1;
  position: relative;
  margin: auto;
  top: 0; bottom: 0;
  padding-left: 10px;
}
.desc-line {
  color: gray;
  font-size: 50%;
  padding-left: 20px;
  text-overflow: ellipsis;
}
.data {
  flex: 1;
  position: relative;
  margin: auto;
  top: 0; bottom: 0;
}
.key,
.value {
  text-overflow: ellipsis;
  white-space: nowrap;
}
</style>
