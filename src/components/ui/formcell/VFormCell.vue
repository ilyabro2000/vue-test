<template>
  <div :class="$style.VFormCell">
    <label v-if="$slots.label" :class="$style.label">
      <slot name="label"/>
      <span v-if="required" :class="$style.star">*</span>
    </label>

    <div :class="[$style.body, { [$style._bordered]: bordered }]">
      <slot name="default"/>
    </div>

    <transition name="error">
      <div v-if="$slots.error" :class="$style.error">
        <slot name="error"/>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'VFormCell',

  props: {
    bordered: {
      type: Boolean,
      default: false,
    },

    required: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style lang="scss" module>
  .VFormCell {
    position: relative;
  }

  .label {
    display: inline-block;
    margin-bottom: 1rem;
    font-size: 1.6rem;
    line-height: 1.5rem;
    color: $font-color;
  }

  .star {
    color: $red;
  }

  .error {
    position: absolute;
    bottom: -3.6rem;
    left: 0;
    margin-bottom: 1.6rem;
    font-size: 1.2rem;
    line-height: 1.5rem;
    color: $red;
    opacity: .9;
  }
</style>
