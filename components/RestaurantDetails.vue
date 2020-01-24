<template>
<div v-if="active">
  {{restaurant.name}}
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
      console.log('ID', id)
      return axios.get(`${this.url}restaurant?res_id=${id}&apikey=${this.apiKey}`).then(({ data }) => {
        console.log('DATA', data)
        this.restaurant = data
        this.active = true
      })
    }
  }
}
</script>
