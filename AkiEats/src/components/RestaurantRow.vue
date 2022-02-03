<template>
  <div class="restaurant--row">
      <h2 class="title">
          Restaurants
      </h2>
        <div class="wrapper--card">
            <restaurant-card v-for="(card, index) in three_restaurant" :info_restaurant="card" :key="index"/>
        </div>
  </div>
</template>

<script>
import RestaurantCard from './RestaurantCard.vue'
export default {
    name: "RestaurantRow",
    components: {
        RestaurantCard,
    },
    props: {
        three_restaurant: Array
    }
}
</script>

<style>
.restaurant--row .wrapper--card {
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.restaurant--row .title {
    font-size: 2rem;
}
</style>