<template>
  <div>
    <h1>Example</h1>
    <vue-carousel
      class="carousel"
      :items="items"
      :current-item="current"
      :visible-at-a-time="[1]"
      :speed="1"
      @carousel-move="updateCurrentItem"
    >
      <template v-slot:item="{ item }">
        <div class="carousel__item">{{ item }}</div>
      </template>
    </vue-carousel>
    <button @click="updateCurrentItem(-1)">&langle;</button>
    <button @click="updateCurrentItem(1)">&rangle;</button>
  </div>
</template>

<script>
import VueCarousel from "@/components/VueCarousel.vue";
import "@/swipeEvents";

export default {
  components: { VueCarousel },
  data() {
    return {
      items: [
        "0",
        "1",
        "2",
        "3",
        "4",
        "5",
        "6",
        "7",
        "8",
        "9",
        "10",
        "11",
        "12"
      ],
      current: 0
    };
  },
  methods: {
    updateCurrentItem(dir) {
      const newPos = this.current + dir;

      if (dir < 0) {
        this.current = newPos >= 0 ? newPos : 0;

        return;
      }

      this.current =
        newPos < this.items.length ? newPos : this.items.length - 1;
    }
  }
};
</script>

<style scoped>
.carousel {
  width: 50%;
  margin: 0 auto;
  background-color: lightcoral;
}

.carousel__item {
  background-color: darkmagenta;
  width: 100px;
  height: 100px;
}
</style>
