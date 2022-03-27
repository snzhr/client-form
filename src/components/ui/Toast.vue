<template>
  <div
    class="toast"
    :style="{
      'background-color': valid ? 'tomato' : '#00ff7f',
      color: valid ? 'white' : 'black',
    }"
  >
    <slot></slot>
  </div>
</template>

<script>
export default {
  props: {
    valid: Boolean,
  },
};
</script>

<style lang="scss" scoped>
.toast {
  background-color: #00ff7f;
  width: fit-content;
  padding: 0 1.5em;
  border-radius: 0.1em;
  position: fixed;
  top: 2em;
  right: 3em;
  box-shadow: rgba(14, 30, 37, 0.12) 0px 2px 4px 0px,
    rgba(14, 30, 37, 0.32) 0px 2px 16px 0px;

  .close {
    position: absolute;
    right: 5px;
    top: 0;
    cursor: pointer;
  }
}
</style>
