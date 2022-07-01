<template>
  <input
    :value="innerValue"
    v-bind="$attrs"
    :disabled="disabled"
    :maxlength="maxLength"
    :class="[$style.VInput, ...classList]"
    @input="onInput"
    @paste="onInput"
    @change="onChange"
    @keydown.enter="onEnter"
    @blur="onBlur"
    @focus="onFocus"
  />
</template>

<script>
export default {
  name: 'VInput',

  props: {
    value: {
      type: [String, Number],
      default: '',
    },

    disabled: {
      type: Boolean,
      default: false,
    },

    error: {
      type: Boolean,
      default: undefined,
    },

    maxLength: {
      type: Number,
      default: 524288,
    },
  },

  data() {
    return {
      innerValue: '',
    };
  },

  computed: {
    classList() {
      return [
        {
          [this.$style._disabled]: this.disabled,
          [this.$style._error]: this.error,
        },
      ];
    },
  },

  watch: {
    value: {
      immediate: true,
      handler(value) {
        this.innerValue = value;
      },
    },
  },

  methods: {
    onInput(e) {
      this.innerValue = e.target.value;

      setTimeout(() => {
        this.$emit('input', this.innerValue);
        this.$emit('origin-input', e);
      }, e.composed ? 1 : 0);

      if (this.error) {
        this.$emit('update:error', null);
      }
    },

    onChange(e) {
      this.innerValue = e.target.value;

      this.$emit('change', this.innerValue);
      this.$emit('origin-change', e);
    },

    onBlur(e) {
      this.innerValue = e.target.value;

      const clearValue = this.innerValue.replace(/\s/g, '');
      if (!clearValue?.length) {
        this.innerValue = '';
      }

      this.$emit('blur', e.target.value);
    },

    onFocus(e) {
      this.$emit('focus', e);
    },

    onEnter(e) {
      setTimeout(() => {
        e.target.blur();
      }, 100);
    },
  },
};
</script>

<style lang="scss" module>
.VInput {
  display: block;
  width: 100%;
  padding: .1rem;
  border: none;
  background: transparent;
  outline: none;
  font-family: $base-font;
  font-size: 1.6rem;
  font-weight: 600;
  line-height: 2rem;
  box-shadow: none;
  transition: color $default-transition;
  cursor: pointer;

  &:hover {
    &::placeholder {
      color: $font-color;
      transition: color $default-transition;
    }
  }

  &::placeholder {
    font-size: 1.6rem;
    font-weight: 600;
    line-height: 2rem;
    color: $additional-color;
    opacity: .44;
    transition: color $default-transition;
  }

  &._disabled {
    color: $light-gray;
  }
}
</style>
