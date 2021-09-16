<template>
  <v-tooltip top>
    <template v-slot:activator="{ on, attrs }">
      <v-scale-transition origin="center">
        <v-btn
          v-show="visible"
          fab
          color="primary"
          :style="customStyle"
          class="back-to-ceiling"
          @click="backToTop"
          v-bind="attrs"
          v-on="on"
          transition="scale-transition"
          origin="center center"
          small
        >
          <v-icon>mdi-arrow-up-bold</v-icon>
        </v-btn>
      </v-scale-transition>
    </template>
    <span>返回顶部</span>
  </v-tooltip>
</template>

<script>
export default {
  name: "BackToTop",
  props: {
    visibilityHeight: {
      type: Number,
      default: 400,
    },
    backPosition: {
      type: Number,
      default: 0,
    },
    customStyle: {
      type: Object,
      default: function () {
        return {
          right: "50px",
          bottom: "50px",
          // width: "40px",
          // height: "40px",
          // "border-radius": "4px",
          // "line-height": "45px",
          // background: "#e7eaf1"
        };
      },
    },
    transitionName: {
      type: String,
      default: "fade",
    },
  },
  data() {
    return {
      visible: false,
      interval: null,
      isMoving: false,
    };
  },
  mounted() {
    window.addEventListener("scroll", this.handleScroll);
  },
  beforeDestroy() {
    window.removeEventListener("scroll", this.handleScroll);
    if (this.interval) {
      clearInterval(this.interval);
    }
  },
  methods: {
    handleScroll() {
      this.visible = window.pageYOffset > this.visibilityHeight;
    },
    backToTop() {
      if (this.isMoving) return;
      const start = window.pageYOffset;
      let i = 0;
      this.isMoving = true;
      this.interval = setInterval(() => {
        const next = Math.floor(this.easeInOutQuad(10 * i, start, -start, 500));
        if (next <= this.backPosition) {
          window.scrollTo(0, this.backPosition);
          clearInterval(this.interval);
          this.isMoving = false;
        } else {
          window.scrollTo(0, next);
        }
        i++;
      }, 16.7);
    },
    easeInOutQuad(t, b, c, d) {
      if ((t /= d / 2) < 1) return (c / 2) * t * t + b;
      return (-c / 2) * (--t * (t - 2) - 1) + b;
    },
  },
};
</script>

<style scoped>
.back-to-ceiling {
  position: fixed;
  z-index: 9999;
}
</style>
