# vue-carousel-generic

## Installation:

```
npm i vue-carousel-generic
```

## Usage example:

```html
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
import VueCarousel from "vue-carousel-generic";

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
```

## Props

- `items` - **required** - a list of elements to fill a carousel with;
- `current-item` - **required** the position number of the currently displayed item
 (in case more than one item is visible at a time, `currentNumber` represents
 the position number of the first of visible elements);
- `visible-at-aTime` - **default:** `[1]` - a list of numbers which represent element's fractions displayed
 in carousel in one slide (e.g. `[0.5, 1, 0.5]` means that on each slide there is half
 of the first element, full second element, half of the third element).
- `speed` - **default:** `1` - the speed of the carousel;
- `swipe-threshold` - **default:** `20` - the threshold in `px` for swipe events to fire;
- `swipe-timeout` - **default:** `500` - the maximum length in `ms` of a single swipe;
- `touch-only-swipes` - **default:** `false` - if `true`, swipe events will only fire on
 touch devices
 
 ## Slots
 
 - `item` - a single element in carousel (slot props: `{ item: any }`)

## Events

- `carousel-move` - Payload: `-1 | 1`
   * `-1` go back one slide
   * `1` go forward one slide
