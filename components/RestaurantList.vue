<template>
<div class="flex">
  {{bookingsAvailable}}
  <div class="w-2/6">
    <div class="fixed w-2/6 inset-0 overflow-auto bg-gray-100">
      <h2 class="pl-12 py-4 mr-4 border-b border-gray-200 text-xs uppercase">Results</h2>
      <div v-for="(r, rIndex) in restaurantList"
           :key="rIndex">
        <div class="pl-12 py-4 mr-4 border-b border-gray-200 solid text-sm"
             @click="showDetails({id: r.id})">
          {{r.name}}
        </div>
      </div>
    </div>
  </div>

  <restaurant-details class="w-4/6"
                      ref="details"></restaurant-details>
</div>
</template>

<script>
import axios from 'axios'
import { map, find } from 'lodash'

import RestaurantDetails from './RestaurantDetails'

export default {
  name: 'List',
  components: { RestaurantDetails },
  data() {
    return {
      restaurants: null,
      url: process.env.ZOMATO_BASE_URL,
      apiKey: process.env.API_KEY
    }
  },
  mounted() {
    this.getRestaurants()
  },
  methods: {
    getRestaurants() {
      return axios.get(`${this.url}search?entity_id=297&entity_type=city&start=1&count=20&apikey=${this.apiKey}`).then(({ data }) => {
        this.restaurants = data.restaurants
      })
    },
    showDetails({ id }) {
      this.$refs.details.show(id)
    }
  },
  computed: {
    restaurantList() {
      return map(this.restaurants, r => {
        return {
          id: r.restaurant.id,
          name: r.restaurant.name,
          location: r.restaurant.location
        }
      })
    },
    bookingsAvailable() {
      console.log('FIND(THIS.RESTAURANTS, R => R.HAS_TABLE_BOOKING)', find(this.restaurants, r => r.has_table_booking === -1))
      return find(this.restaurants, r => r.has_table_booking)
    }
  }
}
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
  @apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
</style>
