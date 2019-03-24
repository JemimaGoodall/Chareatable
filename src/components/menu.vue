// so here: request
<template>
  <div class="splash">
    <div v-if="user">
      <main class="container">
        <div class="jumbotron">
          <form>
            <div class="mb-3">
              <textarea class="form-control" rows="1" id="search" v-model="address" @input="emptyCheck()"></textarea>
            </div>
          </form>
        </div>
        <div class="container" v-if="searchComplete">
          <div class="jumbotron">
            <div v-for="item in items" :key="item.id" class="card mb-4 box-shadow">
              <div class="card-header">
                <h2 class="my-0 font-weight-normal">{{item.name}}</h2>
                <h2 class="my-0 font-weight-normal">{{item.description}}</h2>
                <h2 class="my-0 font-weight-normal">{{item.price}}</h2>
              </div>
              <div class="card-body">
                <router-link :to="{ name: 'order', params: {apikey: apikey, menuapi : item.apiKey} }">
                  <button class="btn btn-lg btn-block btn-primary">Order</button>
                </router-link>
              </div>
            </div>
          </div>

        </div>
      </main>
    </div>
    <div v-else id="firebaseui-auth-container"></div>
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
    searchComplete : false
    }
  },
  methods: {

    async getMenu() {
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
      this.searchComplete = true;
    }
  },

  async mounted () {
    this.getMenu();
  }
}
</script>
