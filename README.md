# vue-carousel-generic

## Usage example:

```html
<template>
    <vue-carousel
        class="carousel"
        :items="items"
        :current-item="current"
        :visible-at-a-time="[0.5, 1, 1, 0.5]"
        :speed="5"
    >
        <template v-slot:item="{ title, content }">
            <div class="carousel__item">
                <h2>{{ title }}</h2>
                <p>{{ content }}</p>
            </div>
        </template>
    </vue-carousel>
    
    <vue-pagination
      class="pagination"
      :current-item="3"
      :visible-at-a-time="[1, 1, 1]"
    >
        <template v-slot:prev>
            <div class="pagination__prev-btn">
                prev
            </div>
        </template>
        <template v-slot:next>
            <div class="pagination__next-btn">
                next
            </div>
        </template>
        <template v-slot:first>
            <div class="pagination__first-btn">
                first
            </div>
        </template>
        <template v-slot:last>
            <div class="pagination__last-btn">
                last
            </div>
        </template>
        <template v-slot:page="{ pageNumber }">
            <div class="pagination__page">
              {{ pageNumber }}
            </div>
        </template>
    </vue-pagination>
</template>


<script>
const options = {
  data() {
    return {
      current: 0,
      items: [
        { title: 'A', content: 'aaa' },
        { title: 'B', content: 'bbb' },
        { title: 'C', content: 'ccc' },
      ]
    };
  },
  methods: {
    onNextBtnClick() {
      this.currentItem = current + 1
    },
    onPrevBtnClick() {
      this.currentItem = current - 1  
    },
    onGoToFirst() {
      this.currentItem = 0
    },
    onGoToLast() {
      this.currentItem = items.length - 1
    },
    onGoToItem(number) {
      this.currentItem = number;
    }
  }
};
</script>


```