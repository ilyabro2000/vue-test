<template>
  <transition
    name="modal"
    mode="out-in"
    duration="500"
  >
    <div v-if="isOpen"
         :class="$style.VModal"
    >
      <div
        :class="$style.wrapper"
        @click.self="close"
      >
        <div :class="$style.container">
          <div :class="$style.header">
            <h5 :class="$style.title">
              Добавление пользователя
            </h5>
            <VExitButton @click="close"/>
          </div>
          <slot/>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import VExitButton from '@/components/ui/button/VCloseButton.vue';

export default {
  name: 'VModal',

  components: {
    VExitButton,
  },

  props: {
    isOpen: {
      type: Boolean,
      default: false,
    },
  },

  methods: {
    close() {
      this.$emit('close');
    },
  },
};
</script>

<style lang="scss" module>

.VModal {
  &:global(.modal-enter-active),
  &:global(.modal-leave-to) {
    &:after {
      opacity: 1;
      transition: opacity .3s ease;
    }

    .container {
      opacity: 1;
      transform: translate3d(0, 0, 0);
      transition: opacity .2s ease .3s, transform .2s ease .3s;
    }
  }

  &:global(.modal-enter),
  &:global(.modal-leave-active) {
    &:after {
      opacity: 0;
      transition: opacity .3s ease .2s;
    }

    .container {
      opacity: 0;
      transform: translate3d(0, -100%, 0);
      transition: opacity .2s ease, transform .2s ease;

      @include respond-to(tablet) {
        transform: translate3d(100%, 0, 0);
      }

      @include respond-to(mobile) {
        transform: translate3d(0, 100%, 0);
      }
    }
  }

  &:after {
    content: "";
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 1;
    width: 100vw;
    height: 100vh;
    background-color: rgba(#000, .4);
    transition: $default-transition;
  }
}

.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 2rem;
  font-size: 1.8rem;
  line-height: 100%;
  font-weight: 600;
  background-color: $bg-color;
}

.wrapper {
  position: relative;
  z-index: 2;
  display: flex;
  align-items: flex-start;
  justify-content: flex-end;
  width: 100vw;
  height: 100vh;
}

.container {
  display: flex;
  flex-direction: column;
  overflow-y: auto;
  width: 50rem;
  margin: 25rem auto 0 auto;

  background-color: white;

  @include respond-to(tablet) {
    width: 40rem;
    height: 100%;
    margin: 0;
  }

  @include respond-to(mobile) {
    display: unset;
    position: absolute;
    bottom: 0;
    margin: 0;
    width: 100vw;
    height: unset;
  }
}
</style>
