// so here: request
<template>
    <div v-if="user">
      <main class="container">
          <div class="jumbotron">
            <div v-for="item in items" :key="item.id" class="card mb-4 box-shadow">
              <div class="card-header">
                <h2 class="my-0 font-weight-normal">{{item.name}}</h2>
                <h2 class="my-0 font-weight-normal">{{item.description}}</h2>
                <h2 class="my-0 font-weight-normal">{{item.basePrice}}</h2>
              </div>
              <div class="card-body">
                <router-link :to="{ name: 'order', params: {apikey: apikey, menuapi: item.apiKey, menuname: item.name, menuprice: item.basePrice, restname: restname} }">
                  <button class="btn btn-lg btn-block btn-primary">Order</button>
                </router-link>
              </div>
            </div>
          </div>
      </main>
    </div>
</template>
<script>
import axios from 'axios'
export default {
  computed: {
    user () {
      return this.$store.state.user
    }
  },
  data(){
    return {
      apikey : this.$route.params.apikey,
      restname: this.$route.params.restname,
      items : null
    }
  },
  async mounted () {
    axios.get('https://eatstreet.com/publicapi/v1/restaurant/'+ this.apikey +'/menu?access-token=41b80af99d9b4d26')
    .then(response => {
      const data = response.data;
      const items = data.map(x => x.items).reduce((acc, x) => acc.concat(x), [])
      this.items = items
    })
    .catch(e => {
      console.log('caught error')
      this.errors.push(e)
    })
  },
}
</script>
