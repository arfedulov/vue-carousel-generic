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
    <vue-pagination :current-page="current" :pages-total-count="items.length" :visible-at-a-time="5" :speed="2" class="pagination" @page-change="setCurrent">
      <template v-slot:page="{ pageNumber }">
        <div class="pagination__page" :class="{ 'pagination__page--current': pageNumber === current }">{{ pageNumber }}</div>
      </template>
    </vue-pagination>
  </div>
</template>

<script>
import VueCarousel from "@/components/VueCarousel.vue";
import VuePagination from "@/components/VuePagination.vue";
import "@/swipeEvents";

export default {
  components: { VueCarousel, VuePagination },
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
    },
    setCurrent(i) {
      this.current = i;
    }
  }
};
</script>

<style scoped>
.carousel {
  width: 50%;
  margin: 0 auto;
}

.carousel__item {
  background-color: darkmagenta;
  width: 100px;
  height: 100px;
}

.pagination {
  width: 30%;
  margin: 0 auto;
}

.pagination__page {
  width: 30px;
  height: 30px;
  background-color: red;
}

.pagination__page--current {
  background-color: blue;
}
</style>
