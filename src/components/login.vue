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
        <button :disabled="empty" class="btn btn-primary btn-lg btn-block col-md-3" type="submit" @click="getRestaurants()">Search</button>
        </div>
        <div class="container" v-if="searchComplete">
          <div class="jumbotron">
              <div v-for="restaurant in resto" :key="restaurant.id" class="card mb-4 box-shadow">
            <div class="card-header">
              <h2 class="my-0 font-weight-normal">{{restaurant.name}}</h2></div>
            <div class="card-body">
              <router-link :to="{ name: 'menu', params: {apikey: restaurant.apiKey} }">
                <button class="btn btn-lg btn-block btn-primary">View menu</button>
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
import firebase from 'firebase'
import firebaseui from 'firebaseui'
import db from '@/firebase/init.js'
import axios from 'axios'
export default {
  computed: {
    user () {
      return this.$store.state.user
    }
  },
  data(){
    return {
      unameCheck: null,
      address: null,
      empty: null,
      apiResponse: null,
      resto: [],
      databaseData: null,
      searchComplete: false
    }
  },
  methods:{
    profilelink: function(){
      return "/profile/"+this.user.uname
    },
    emptyCheck: function(){
      if (this.address == null || this.address == "") {
        this.empty = true
        return
      }
      else {
        this.empty = false
        return
      }
    },
    async getRestaurants(){
      axios.get('https://eatstreet.com/publicapi/v1/restaurant/search?access-token=41b80af99d9b4d26&street-address='+this.address)
    .then(response => {
      this.apiResponse = response.data.restaurants;
    })
    .catch(e => {
      this.errors.push(e)
    })
    let restList = await db.collection('fundraisers').get()
    this.databaseData = restList.docs.map(x => x.data().name);
    for (var i = 0; i < this.apiResponse.length; i++) {
      if (this.databaseData.includes(this.apiResponse[i].name)) {
        this.resto.push(this.apiResponse[i]);
      }
    }
    this.searchComplete = true;
    },
  },
  async mounted () {
    if (!this.user) {
      var ui = new firebaseui.auth.AuthUI(firebase.auth())
      ui.start('#firebaseui-auth-container', {
        signInSuccessUrl: '/',
        signInOptions: [firebase.auth.GoogleAuthProvider.PROVIDER_ID,firebase.auth.EmailAuthProvider.PROVIDER_ID]
      })
    }
    if (this.user.uname == undefined) {
      this.$router.push({ name: "editprofile"})
    }
    else {
      if (this.address == null || this.address == "") {
        this.empty = true
        return
      }
      else {
        this.empty = false
        return
      }
    }
  }
}
</script>

<style>
.splash{
  text-align: center;
}
.material-icons.md-18 { font-size: 18px; }
.material-icons.md-24 { font-size: 24px; }
.material-icons.md-36 { font-size: 36px; }
.material-icons.md-48 { font-size: 48px; }

.spinner-wrapper {
  margin-left: 49%;
}
</style>
