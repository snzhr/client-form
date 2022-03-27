<template>
  <label class="form-item" :for="id">
    <span>{{ label }}</span>
    <input
      :value="value"
      @input="$emit('input', $event.target.value)"
      :type="type"
      :id="id"
      :class="{ error: isInvalid }"
    />
    <slot></slot>
  </label>
</template>

<script>
export default {
  props: {
    id: String,
    type: String,
    required: Boolean,
    label: String,
    value: [String, Number],
    isInvalid: Boolean,
  },
};
</script>

<style lang="scss" scoped>
@import "@/assets/_common.scss";

.form-item {
  @include form-item-style;

  input {
    @include font-styles;
    padding: 0.3em;
    font-size: 16px;
    border: 1px solid gainsboro;
    border-radius: 0.2em;
    background-color: $bg-color;
    margin-top: 0.5em;
    &:focus {
      @include field-focus;
    }
  }
  .error-text {
    color: red !important;
    font-size: 14px;
    margin-top: 0.4em;
  }
  .error {
    box-shadow: tomato 0px 0px 0px 2px !important;
  }
}
</style>
