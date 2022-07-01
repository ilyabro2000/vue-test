<template>
  <VButton :class="$style.VCloseButton"
           square
           @mouseover.native="isHovered = true"
           @mouseout.native="isHovered = false"
           @click="$emit('click')"
  >
    <div :class="[$style.close, { [$style.hovered]: isHovered }]"/>
  </VButton>
</template>

<script>
import VButton from '@/components/ui/button/VButton.vue';

export default {
  name: 'VCloseButton',

  components: {
    VButton,
  },

  data() {
    return {
      isHovered: false,
    };
  },
};
</script>

<style lang="scss" module>
  .VCloseButton {
    position: relative;
    height: 2.5rem;
    width: 2.5rem;
    padding: .5rem;
  }

  .close {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 100%;

    &:before,
    &:after {
      position: absolute;
      top: .15rem;
      right: 0;
      bottom: 0;
      left: 1.2rem;
      content: " ";
      height: 2rem;
      width: .22rem;
      background-color: $additional-color;
      transition: $default-transition;
    }
    &:before {
      transform: rotate(45deg);
    }
    &:after {
      transform: rotate(-45deg);
    }

    &.hovered {
      &:before,
      &:after {
        background-color: white;
        transition: $default-transition;
      }
    }
  }
</style>
