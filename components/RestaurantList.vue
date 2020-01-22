<template>
<div class="flex flex-col fixed inset-0 h-full w-2/6 overflow-auto bg-gray-100">
  <h2 class="pl-12 py-4 mr-4 border-b border-gray-200 text-xs uppercase">Results</h2>
  <div v-for="(r, rIndex) in restaurantList"
       :key="rIndex">
    <div class="pl-12 py-4 mr-4 border-b border-gray-200 solid text-sm">
      {{r.name}}
    </div>
  </div>
</div>
</template>

<script>
import axios from 'axios'
import { map } from 'lodash'

export default {
  name: 'List',
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
      return axios.get(`${this.url} + entity_id=297&entity_type=city&start=1&count=50 + &apikey=${this.apiKey}`).then(({ data }) => {
        this.restaurants = data.restaurants
      })
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
