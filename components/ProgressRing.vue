<template>
<div>
  <div class="circle-progress" :style="{width:size+'px',height:size+'px'}" :data-pct="`${progressNumber === 100 ? '✅' : progressNumber + '%'}`">

    <!-- <component :is="'style'">
      .circle-progress:after {
        color: {{ invertColor(primaryColor, true) }};
      }
    </component> -->
    <svg :width="size" :height="size" :viewPort="'0 0 '+ size + ' ' + size" version="1.1" xmlns="http://www.w3.org/2000/svg">
      <circle class="ring"  stroke-opacity="0.4" :stroke="ringColor" :r="r" :cx="size/2" :cy="size/2" :stroke-width="strokeWidth" fill="none">
      </circle>
      <circle class="progress_circle" :stroke="progressColor" :r="r" :cx="size/2" :cy="size/2" :stroke-width="strokeWidth" fill="none" :stroke-dasharray="dasharray" :stroke-dashoffset="dashoffset"></circle>
    </svg>
  </div>
</div>
</template>
<script>
export default {
  name: "circle-progress",
  data() {
    return {};
  },
  props: {
    size: {
      type: Number,
      default: 32
    },
    percentage: {
      type: Number,
      default: 0,
      validator: val => {
        return val <= 100;
      }
    },
    strokeWidth: {
      type: Number,
      default: 3
    },
    ringColor: {
      type: String,
      default: "#E9EBEE"
    },
    progressColor: {
      type: String,
      default: "#13b30a"
    },
    showNumber: {
      type: Boolean,
      default: true
    }
  },
  computed: {
    r() {
      return this.size / 2 - this.strokeWidth / 2;
    },
    dasharray() {
      return 2 * Math.PI * this.r;
    },
    primaryColor() {
      return this.$store.state.studentView.primaryColor
    },
    dashoffset() {
      return this.dasharray * (1 - this.percentage / 100);
    },
    progressNumber() {
      return this.showNumber && parseInt(this.percentage);
    }
  },
  methods: {
    
  }
};
</script>
<style lang="css" scoped>
.circle-progress {
  position: relative;
  text-align: center;
}
.ring {
  stroke-opacity: 0.4;
}
.circle-progress:after {
  content: attr(data-pct);
  font-size:12px;
  font-weight: 500;
  width:100%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate3d(-50%, -50%, 0);
}
.circle-progress svg {
  transform: rotate(-90deg);
}
.circle-progress .progress_circle {
  stroke-linecap: round;
  transition: stroke-dashoffset 1s linear;
}
.circle-percentage {
  font-size: 75%;
}
</style>
