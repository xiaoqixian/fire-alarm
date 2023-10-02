<template>
  <div :style="divStyle"><button :disabled="disabled"><slot/></button></div>
</template>

<script>
export default {
  name: 'MyButton',
  props: {
    disabled: Boolean,
    divStyle: {
      backgroundColor: String,
      backgroundPosition: String,
      backgroundImage: String,
      aspectRatio: String
    }
  },
}
</script>

<style>
button {
  width: 100%;
  height: 100%;
  background-color: transparent;
}
</style>
