<template>
  <perfect-scrollbar ref="scrollbar" :options="options">
    <slot></slot>
  </perfect-scrollbar>
</template>

<script>
import { PerfectScrollbar } from 'vue2-perfect-scrollbar';
import 'vue2-perfect-scrollbar/dist/vue2-perfect-scrollbar.css';

export default {
  name: 'VScrollBox',

  components: {
    PerfectScrollbar,
  },

  data() {
    return {
      options: {
        useBothWheelAxes: false,
        suppressScrollX: true,
        wheelPropagation: false,
      },
    };
  },
};
</script>

<style src="vue2-perfect-scrollbar/dist/vue2-perfect-scrollbar.css"/>
