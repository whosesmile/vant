<template>
  <div :class="b()">
    <span :class="b('portion', { 'with-pivot': showPivot })" :style="portionStyle">
      <span v-if="showPivot" ref="pivot" :style="pivotStyle" :class="b('pivot')">{{ pivotText }}</span>
    </span>
  </div>
</template>

<script>
import create from '../utils/create';

export default create({
  name: 'progress',

  props: {
    inactive: Boolean,
    pivotColor: String,
    percentage: {
      type: Number,
      required: true,
      validator: value => value >= 0 && value <= 100
    },
    showPivot: {
      type: Boolean,
      default: true
    },
    pivotText: {
      type: String,
      default() {
        return this.percentage + '%';
      }
    },
    color: {
      type: String,
      default: '#38f'
    },
    textColor: {
      type: String,
      default: '#fff'
    }
  },

  data() {
    return {
      pivotWidth: 0,
      progressWidth: 0
    };
  },

  computed: {
    currentColor() {
      return this.inactive ? '#cacaca' : this.color;
    },

    pivotStyle() {
      return {
        color: this.textColor,
        background: this.pivotColor || this.currentColor
      };
    },

    portionStyle() {
      return {
        width: (this.progressWidth - this.pivotWidth) * this.percentage / 100 + 'px',
        background: this.currentColor
      };
    }
  },

  mounted() {
    this.getWidth();
  },

  watch: {
    showPivot() {
      this.getWidth();
    },

    pivotText() {
      this.getWidth();
    }
  },

  methods: {
    getWidth() {
      this.progressWidth = this.$el.offsetWidth;
      this.pivotWidth = this.$refs.pivot ? this.$refs.pivot.offsetWidth : 0;
    }
  }
});
</script>
