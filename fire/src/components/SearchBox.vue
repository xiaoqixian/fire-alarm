<template>
  <div class="search-box" :style="{width: width, height: height, border: border}">
    <input class="search-input" v-model="searchText" :placeholder="placeholder"/>
    <div class="search-button" :style="{backgroundImage: backgroundImage}">
      <button @click="clickAction"/>
    </div>
</div>
</template>

<script>
export default {
  props: {
    placeholder: {
      type: String,
      default: ""
    },
    backgroundImage: {
      type: String,
      required: true
    },
    clickAction: {
      type: String,
      required: true
    }
  }
}
</script>

<style>
.search-box {
  display: flex;
}
.search-input {
  flex: auto;
  border: none;
  background-color: inherit;
  color: inherit;
  text-overflow: ellipsis;
  line-height: 100%;
  font-size: 70%;
}
.search-button {
  flex: none;
  justify-content: flex-end;
  aspect-ratio: 1/1;
  height: 100%;
  background-size: 80%;
  background-position: center;
  background-repeat: no-repeat;
  border: none;
}
.search-button button {
  width: 100%;
  height: 100%;
  background-color: transparent;
}
</style>
