<template>
  <div class="home">
      <div class="header">
          <img src="https://d3i4yxtzktqr9n.cloudfront.net/web-eats-v2/ee037401cb5d31b23cf780808ee4ec1f.svg" alt="Aki-ubereats">
          <div class="wrapper--input">
            <input v-model="user_search_restaurant" type="text" placeholder="Une petite faim ?">
            <div class="search">
                <router-link v-for="(restaurant, index) in search_restaurant" :key="index"  :to="{ name: 'Restaurant', params: {name: restaurant.name }}">
                    <div class="container--restaurant--search">
                    <div class="wrapper--img">
                        <img :src="restaurant.image" alt="">
                    </div>
                    <p>{{ restaurant.name }}</p>
                </div>
                </router-link>
            </div>
          </div>
      </div>
      <div class="banner">

      </div>
      <restaurant-row v-for="(data, index) in data_restaurant" :key="index" :three_restaurant="data"/>
  </div>
</template>

<script>
import BDD from '../BDD.js'
import { onMounted, ref, watch } from 'vue'
import RestaurantRow from '../components/RestaurantRow.vue'
export default {
    name: "Home",
        components: {
            RestaurantRow,
        },
        setup() {
            class Restaurant {
                constructor (name, note, image, drive_time) {
                    this.name = name,
                    this.note = note,
                    this.image = image,
                    this.drive_time = drive_time
                }
            }

            let data_restaurant = ref([])
            let all_restaurant = []

            const makeDataRestaurant = () => {

                let three_restaurant = []

                for (const restaurant of BDD) {

                    const new_restaurant = new Restaurant(restaurant.name, restaurant.note, restaurant.image, restaurant.drive_time)

                    all_restaurant.push(new_restaurant)

                    if (three_restaurant.length === 2) {
                        three_restaurant.push(new_restaurant);
                        data_restaurant.value.push(three_restaurant);
                        three_restaurant = [];
                    } else {
                        three_restaurant.push(new_restaurant);
                    }
                }
            }

            let user_search_restaurant = ref('');
            let search_restaurant = ref([]);

            watch(user_search_restaurant, new_val => {
                
                let regex = RegExp(new_val.toLowerCase());

                let new_search_restaurant = all_restaurant.filter(restaurant => regex.test(restaurant.name.toLowerCase()));


                new_val == 0 ? search_restaurant.value = [] : search_restaurant.value = new_search_restaurant
            })

            onMounted(makeDataRestaurant);

            return {
                data_restaurant,
                user_search_restaurant,
                search_restaurant
            }
        }
}
</script>

<style>
.home .header {
    height: 120px;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.home .header img {
    width: 200px;
}

.home .header .wrapper--input {
    position: relative;
}

.home .header .wrapper--input input {
    background-color: #F6F6F6;
    border: none;
    height: 60px;
    width: 400px;
    outline: none;
    padding-left: 20px;
}

.home .header .wrapper--input .search {
    position: absolute;
    top: 100%;
    width: 100%;
    background-color: #fff;
}

.home .header .wrapper--input .search .container--restaurant--search {
    display: flex;
    align-items: center;
    padding: 10px;
}

.home .header .wrapper--input .search .container--restaurant--search:hover {
    background: #f6f6f6;
}

.home .header .wrapper--input .search .container--restaurant--search .wrapper--img {
    height: 60px;
    width: 60px;
    margin-right: 25px;
    border-radius: 50%;
    overflow: hidden;
}

.home .header .wrapper--input .search .container--restaurant--search .wrapper--img img {
    height: 100%;
    width: 100%;
}

.home .banner {
    height: 200px;
    width: 100%;
    background-image: url("https://www.ubereats.com/restaurant/_static/7b308f7cbbf8e335ceda0447a8bd7c63.png");
    background-size: cover;
    background-position: center center;
}
</style>