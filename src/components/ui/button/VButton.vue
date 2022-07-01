<template>
  <div :class="[$style.VButton, ...classList]"
       @click="$emit('click')"
  >
    <slot/>
  </div>
</template>

<script>
export default {
  name: 'VButton',

  props: {
    disabled: {
      type: Boolean,
      default: false,
    },

    square: {
      type: Boolean,
      default: false,
    },

    color: {
      type: String,
      default: 'transparent',
      validator: (value) => [
        'gray',
        'transparent',
      ].includes(value),
    },
  },

  computed: {
    classList() {
      return [
        this.$style[`_${this.color}`],
        {
          [this.$style._disabled]: this.disabled,
          [this.$style._square]: this.square,
        },
      ];
    },
  },
};
</script>

<style lang="scss" module>
  .VButton {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 1rem 2rem;
    border-radius: 2rem;
    text-align: center;
    font-size: 1.2rem;
    line-height: 100%;
    letter-spacing: .001em;
    text-transform: uppercase;
    background-color: transparent;
    overflow: hidden;
    cursor: pointer;
    opacity: 1;
    transition: $default-transition;

    // colors
    &._gray {
      background-color: $bg-color;
      color: $base-color;
      border: .15rem solid $base-color;

      &:hover,
      &:focus {
        color: white;
        background-color: $base-color;
        transition: $default-transition;
      }
    }

    &._transparent {
      &:hover,
      &:focus {
        color: white;
        background-color: $base-color;
        transition: $default-transition;
      }
    }

    // state
    &._disabled {
      opacity: .44;
      pointer-events: none;
      transition: $default-transition;
    }

    // forms

    &._square {
      border-radius: .5rem;
    }
  }
</style>
