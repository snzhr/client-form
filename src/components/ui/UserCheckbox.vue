<template>
  <label class="form-item" :for="id">
    <span>{{ label }}</span>
    <input
      type="checkbox"
      :id="id"
      :checked="checked"
      @change="$emit('change', $event.target.checked)"
    />
  </label>
</template>

<script>
export default {
  props: {
    id: String,
    label: String,
    checked: Boolean,
  },
  model: {
    prop: "checked",
    event: "change",
  },
};
</script>
<style lang="scss" scoped>
label {
  display: block;
  width: 48%;
  input {
    cursor: pointer;
  }
}
</style>
