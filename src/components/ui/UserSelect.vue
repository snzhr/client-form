<template>
  <label :for="id" class="form-item">
    <span :style="{ marginBottom: '.5em' }">{{ label }}</span>
    <select
      :id="id"
      :value="value"
      @input="$emit('input', $event.target.value)"
      :class="{ error: isInvalid }"
    >
      <option disabled value="" selected>Выбрать...</option>
      <option v-for="d in data" :key="d" :value="d">{{ d }}</option>
    </select>
    <slot></slot>
  </label>
</template>

<script>
export default {
  props: {
    id: String,
    label: String,
    data: Array,
    value: String,
    isInvalid: Boolean,
  },
};
</script>
<style lang="scss" scoped>
@import "@/assets/_common.scss";

.form-item {
  @include form-item-style;

  select {
    @include select-styles;
    @include font-styles;
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
