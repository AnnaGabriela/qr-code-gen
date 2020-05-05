<template>
  <div class="picker"></div>
</template>

<script>
import Pickr from '@simonwep/pickr';

export default {
  name: 'data-picker',
  props: {
    default: {
      type: String,
      required: false,
    },
    options: {
      type: Object,
      required: false,
      default() {
        return {};
      },
    },
  },
  mounted() {
    const pickr = new Pickr({
      el: this.$el,
      default: this.default,
      ...this.options,
      appClass: 'picker',
    });
    pickr.on('save', () => pickr.hide());
    pickr.on('change', (color) => {
      this.$emit('newColor', color.toHEXA().toString());
    });
  },
};
</script>

<style scoped>
@import '~@simonwep/pickr/dist/themes/nano.min.css';

.picker {
border: 2px solid black;
}
</style>
