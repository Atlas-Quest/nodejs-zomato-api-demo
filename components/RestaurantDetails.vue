<template>
<div v-if="active"
     class="flex p-12">
  <div class="flex justify-start">
    <img :src="restaurant.thumb"
         alt="">
  </div>
  <div class="flex flex-col">
    <div class="flex">
      {{restaurant.name}}
    </div>
    <div class="flex">
      {{restaurant.location.address}}
    </div>
    <div v-if="restaurant.has_table_booking"
         class="flex">
      tick
    </div>
    <div v-else
         class="flex">
      cross
    </div>
    <div v-if="restaurant.is_delivering_now"
         class="flex">
      tick
    </div>
    <div v-else
         class="flex">
      cross
    </div>
    <div class="">
      {{restaurant.cuisines}}
    </div>
    <div class="">
      {{restaurant.phone_numbers}}
    </div>
    <div class="">
      {{restaurant.timings}}
    </div>
  </div>
</div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'RestaurantDetails',
  data() {
    return {
      active: false,
      restaurant: null,
      url: process.env.ZOMATO_BASE_URL,
      apiKey: process.env.API_KEY
    }
  },
  methods: {
    show(id) {
      return axios.get(`${this.url}restaurant?res_id=${id}&apikey=${this.apiKey}`).then(({ data }) => {
        this.restaurant = data
        this.active = true
      })
    }
  }
}
</script>
