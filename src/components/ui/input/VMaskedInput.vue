<template>
  <VInput
    ref="input"
    :value="innerValue"
    v-bind="$attrs"
    :disabled="disabled"
    :error="error"
    @origin-input="$emit('origin-input', $event)"
    @input="onInput"
    @change="$emit('change', $event)"
    @blur="$emit('blur', $event)"
    @focus="$emit('focus', $event)"
  />
</template>

<script>
import VInput from '@/components/ui/input/VInput';
import inputmask from 'inputmask/dist/inputmask';

export default {
  name: 'VMaskedInput',

  components: {
    VInput,
  },

  props: {
    value: {
      type: [String, Number],
      default: '',
    },

    mask: {
      type: String,
      default: '+7 999 999 99 99',
    },

    maskPlaceholder: {
      type: String,
      default: ' ',
    },

    showMaskOnHover: {
      type: Boolean,
      default: true,
    },

    disabled: {
      type: Boolean,
      default: false,
    },

    error: {
      type: Boolean,
      default: undefined,
    },
  },

  data() {
    return {
      innerValue: '',
    };
  },

  watch: {
    value: {
      immediate: true,
      handler(value) {
        this.innerValue = value;
      },
    },
  },

  mounted() {
    this.init();
  },

  methods: {
    init() {
      inputmask({
        showMaskOnFocus: this.showMaskOnFocus,
        placeholder: this.maskPlaceholder,
        mask: this.mask,
      })
        .mask(this.$refs.input.$el);
    },

    onInput(e) {
      this.innerValue = e;
      this.$emit('input', this.innerValue);

      if (this.error) {
        this.$emit('update:error', null);
      }
    },
  },
};
</script>
