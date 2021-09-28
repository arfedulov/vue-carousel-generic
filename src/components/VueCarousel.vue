<template>
  <vue-carousel-wrap
    ref="carousel"
    class="vue-carousel"
    :class="carouselClasses"
    :swipe-threshold="swipeThreshold"
    :swipe-timeout="swipeTimeout"
    :touch-only-swipes="touchOnlySwipes"
    @swipe="onSwipe"
  >
    <div
      ref="content"
      class="vue-carousel__content vue-carousel-content"
      :style="contentStyles"
    >
      <template v-for="(item, index) in items">
        <div
          class="vue-carousel-content__item vue-carousel-item"
          :style="itemStyles"
          :key="index"
        >
          <slot name="item" :item="item"></slot>
        </div>
      </template>
    </div>
  </vue-carousel-wrap>
</template>

<script>
import VueCarouselWrap from "@/components/VueCarouselWrap.vue";

export default {
  name: "VueCarousel",
  components: {VueCarouselWrap},
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
    step: {
      type: Number,
      default: 1
    },
    speed: {
      type: [Number, String],
      default: 1,
      validator: value =>
        typeof value === "string" ? value === "instant" : true
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
    },
    swipeTimingFunction: {
      type: String,
      default: 'ease-out'
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
      let transformTime = this.speed === "instant" ? 0 : 1 / this.speed;

      if (first % 1 !== 0) {
        paddingLeft = `${this.itemWidth * first}px`;
      }

      return {
        paddingLeft,
        transform: `translateX(${translateX}px)`,
        transition: `transform ${transformTime}s ${this.swipeTimingFunction}`
      };
    },
    carouselClasses() {
      return {
        'vue-carousel--mouse-swipe-enabled': !this.touchOnlySwipes
      }
    }
  },
  watch: {
    items: {
      handler: 'resetCarouselWidth'
    }
  },
  mounted() {
    this.resetCarouselWidth();

    window.addEventListener("resize", this.resetCarouselWidth);
  },
  beforeDestroy() {
    window.removeEventListener("resize", this.resetCarouselWidth);
  },
  methods: {
    resetCarouselWidth() {
      if (!this.$refs.carousel) {
        return;
      }

      const { width } = this.$refs.carousel.$el.getBoundingClientRect();

      this.carouselWidth = width;
    },
    onSwipe(direction) {
      if (direction === "left") {
        this.$emit("carousel-move", this.step);

        return;
      }

      if (direction === "right") {
        this.$emit("carousel-move", -this.step);
      }
    }
  }
};
</script>

<style scoped>
.vue-carousel {
  overflow-x: hidden;
}

.vue-carousel--mouse-swipe-enabled {
  user-select: none;
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
