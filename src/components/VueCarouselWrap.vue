<template>
  <div
    ref="carousel"
    :data-swipe-threshold="swipeThreshold"
    :data-swipe-timeout="swipeTimeout"
    :data-swipe-with-mouse-ignore="touchOnlySwipes"
  >
    <slot></slot>
  </div>
</template>

<script>
import "@/swipeEvents";

export default {
  name: "VueCarouselWrap",
  props: {
    swipeThreshold: {
      type: Number,
      default: 20
    },
    swipeTimeout: {
      type: Number,
      default: 500
    },
    touchOnlySwipes: {
      type: Boolean,
      default: false
    },
  },
  mounted() {
    this.$refs.carousel.addEventListener("swiped", this.onSwipe);
  },
  beforeDestroy() {
    this.$refs.carousel.removeEventListener("swiped", this.onSwipe);
  },
  methods: {
    onSwipe(event) {
      if (event.detail.dir === "left") {
        this.$emit('swipe', "left")

        return;
      }

      if (event.detail.dir === "right") {
        this.$emit('swipe', "right")
      }
    }
  }
}
</script>
