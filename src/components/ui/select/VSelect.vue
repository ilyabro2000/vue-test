<template>
  <div :class="$style.VSelect"
       v-click-outside="handleClickOutside"
  >
    <div :class="$style.field"
         @click="activeList = !activeList"
    >
      <transition name="fade"
                  :key="activeList"
      >
        <div :class="$style.value">
          <span v-if="value">
            {{ value.name }}
          </span>
          <span :class="$style.dropIcon">
            {{ activeList ? '-' : '+' }}
          </span>
        </div>
      </transition>

    </div>
    <transition name="dropdown">
      <div v-if="activeList"
           :class="$style.listWrapper"
      >

        <VScrollBox :class="$style.options">
          <div v-for="option in options"
               :key="option.id"
               :class="[$style.option, setOptionActiveClass(option)]"
               @click="setValue(option)"
          >
            {{ option.name }}
          </div>
        </VScrollBox>
      </div>
    </transition>
  </div>
</template>

<script>

import VScrollBox from '@/components/ui/scrollbox/VScrollBox';
import ClickOutside from 'vue-click-outside';

export default {
  name: 'VSelect',

  components: {
    VScrollBox,
  },

  props: {
    value: {
      type: Object,
      default: () => ({}),
    },
    options: {
      type: Array,
      default: () => [],
    },
  },

  data() {
    return {
      activeList: false,
    };
  },

  directives: {
    ClickOutside,
  },

  methods: {
    setValue(value) {
      if (value.id === this.value?.id) {
        this.$emit('change', null);
      } else {
        this.activeList = false;
        this.$emit('change', value);
      }
    },

    setOptionActiveClass(option) {
      return this.value ? { [this.$style._active]: option.id === this.value?.id } : '';
    },

    handleClickOutside() {
      this.activeList = false;
    },
  },
};
</script>

<style lang="scss" module>
  .VSelect {
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    width: 100%;
    min-width: 15rem;
    font-size: 1.6rem;
    font-weight: 600;
    line-height: 2rem;
  }

  .listWrapper {
    position: absolute;
    top: 2.8rem;
    left: 0;
    z-index: 5;
    background-color: white;

    @include respond-to(tablet) {
      right: 0;
    }
  }

  .options {
    height: auto;
    max-height: 10rem;
    transition: $default-transition;
  }

  .field {
    width: 100%;
    height: 100%;
    border: 1px solid #e9eff4;
    cursor: pointer;
  }

  .option, {
    width: 20rem;
    height: 3rem;
    padding: .5rem;
    border: 1px solid #e9eff4;
    cursor: pointer;

    &._active {
      background-color: $base-color;
      color: white;
      transition: $default-transition;

      &:hover {
        color: $font-color;
      }
    }

    &:hover {
      background-color: $light-gray;
      transition: $default-transition;
    }
  }

  .dropIcon {
    font-weight: 700;
    position: absolute;
    right: 1rem;
    top: .5rem;
    line-height: 100%;
  }
</style>
