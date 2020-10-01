<template>
  <div ref="pagination" class="vue-carousel-pagination">
    <div class="vue-carousel-pagination__content vue-carousel-pagination-content" :style="contentStyles">
      <template v-for="pageNumber of pages">
        <div :key="pageNumber" :style="pageStyles" class="vue-carousel-pagination-content__page">
          <div @click="onPageChange(pageNumber)">
            <slot name="page" :page-number="pageNumber"></slot>
          </div>
        </div>
      </template>
    </div>
  </div>
</template>

<script>
export default {
  name: "VuePagination",
  props: {
    pagesTotalCount: {
      type: Number,
      required: true
    },
    currentPage: {
      type: Number,
      required: true
    },
    visibleAtATime: {
      type: Number,
      default: Infinity
    },
    speed: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      paginationWidth: 0
    };
  },
  computed: {
    pages() {
      const arr = [];

      for (let i = 0; i < this.pagesTotalCount; i++) {
        arr.push(i);
      }

      return arr;
    },
    pageWidth() {
      const total = this.visibleAtATime === Infinity ? this.pagesTotalCount : this.visibleAtATime;

      return this.paginationWidth / total;
    },
    pageStyles() {
      return { width: `${this.pageWidth}px` };
    },
    contentStyles() {
      let translateX = 0;
      let transformTime = this.speed > 0 ? 1 / this.speed : 0;

      if (this.visibleAtATime !== this.pagesTotalCount) {
        let shiftLeft = 0;

        if (this.currentPage > Math.floor(this.visibleAtATime / 2)) {
          shiftLeft = Math.floor(this.visibleAtATime / 2) * this.pageWidth;
          translateX = (this.currentPage * -this.pageWidth) + shiftLeft;
        }
      }

      return {
        transform: `translateX(${translateX}px)`,
        transition: `transform ${transformTime}s ease`
      };
    }
  },
  mounted() {
    this.resetPaginationWidth();

    window.addEventListener("resize", this.resetPaginationWidth);
  },
  beforeDestroy() {
    window.removeEventListener("resize", this.resetPaginationWidth);
  },
  methods: {
    resetPaginationWidth() {
      const { width } = this.$refs.pagination.getBoundingClientRect();

      this.paginationWidth = width;
    },
    onPageChange(pageNumber) {
      if (pageNumber === this.currentPage) {
        return;
      }

      this.$emit('page-change', pageNumber);
    }
  }
};
</script>

<style scoped>
.vue-carousel-pagination {
  overflow-x: hidden;
  background-color: green;
}

.vue-carousel-pagination-content {
  display: flex;
}

.vue-carousel-pagination-content__page {
  flex: 1 0 auto;
  display: flex;
  justify-content: center;
  cursor: pointer;
}
</style>
