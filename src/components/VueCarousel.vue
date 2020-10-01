<template>
  <div
    ref="carousel"
    class="vue-carousel"
    :data-swipe-threshold="swipeThreshold"
    :data-swipe-timeout="swipeTimeout"
    :data-swipe-with-mouse-ignore="touchOnlySwipes"
  >
    <div
      ref="content"
      class="vue-carousel__content vue-carousel-content"
      :style="contentStyles"
    >
      <template v-for="item in items">
        <div
          class="vue-carousel-content__item vue-carousel-item"
          :style="itemStyles"
          :key="item"
        >
          <slot name="item" :item="item"></slot>
        </div>
      </template>
    </div>
  </div>
</template>

<script>
const CAROUSEL_SPEED = {
  MAX: 5
};

export default {
  name: "VueCarousel",
  props: {
    items: {
      type: Array,
      required: true
    },
    currentItem: {
      type: Number,
      required: true
    },
    visibleAtATime: {
      type: Array,
      default: () => [1],
      validator: values => values.every(value => typeof value === "number")
    },
    speed: {
      type: Number,
      default: CAROUSEL_SPEED.MAX
    },
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
    }
  },
  data() {
    return {
      carouselWidth: 0
    };
  },
  computed: {
    itemWidth() {
      const total = this.visibleAtATime.reduce((acc, el) => acc + el, 0);

      return this.carouselWidth / total;
    },
    itemStyles() {
      return {
        width: `${this.itemWidth}px`,
        cursor: this.touchOnlySwipes ? "default" : "pointer"
      };
    },
    contentStyles() {
      const [first] = this.visibleAtATime;

      let paddingLeft = "0px";
      let translateX = this.currentItem * -this.itemWidth;
      let transformTime = this.speed > 0 ? 1 / this.speed : 0;

      if (first % 1 !== 0) {
        paddingLeft = `${this.itemWidth * first}px`;
      }

      return {
        paddingLeft,
        transform: `translateX(${translateX}px)`,
        transition: `transform ${transformTime}s ease`
      };
    }
  },
  mounted() {
    this.resetCarouselWidth();

    this.$refs.carousel.addEventListener("swiped", this.onSwipe);

    window.addEventListener("resize", this.resetCarouselWidth);
  },
  beforeDestroy() {
    this.$refs.carousel.removeEventListener("swiped", this.onSwipe);

    window.removeEventListener("resize", this.resetCarouselWidth);
  },
  methods: {
    resetCarouselWidth() {
      const { width } = this.$refs.carousel.getBoundingClientRect();

      this.carouselWidth = width;
    },
    onMove(dir) {
      this.$emit("carousel-move", dir);
    },
    onSwipe(event) {
      if (event.detail.dir === "left") {
        this.onMove(1);

        return;
      }

      if (event.detail.dir === "right") {
        this.onMove(-1);

        return;
      }
    }
  }
};
</script>

<style scoped>
.vue-carousel {
  overflow-x: hidden;
  background-color: lightcoral;
}

.vue-carousel__content {
  display: flex;
}

.vue-carousel-content__item {
  flex: 1 0 auto;
}

.vue-carousel-item {
  display: flex;
  justify-content: center;
}
</style>
