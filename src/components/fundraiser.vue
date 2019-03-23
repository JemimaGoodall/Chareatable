<template>
  <main class="container">
    <div class="jumbotron">
      <h1 align="center">{{displayname}}</h1>
      <h4 class="section-head">Description</h4>
      <p class="content">{{description}}</p>
      <h4 class="section-head">City</h4>
      <p class="content">{{city}}</p>
      <h4 class="section-head">State</h4>
      <p class="content">{{stt}}</p>
      <h4 class="section-head">Country</h4>
      <p class="content">{{country}}</p>
      <h4 class="section-head">Fundraising Organizer</h4>
      <p class="content">{{organizer}}</p>
      <h4 class="section-head">Restaurant Name</h4>
      <p class="content">{{restaurant}}</p>
      <router-link to="/edit-fundraiser">
      <button v-if="userCheck" class="btn btn-primary btn-lg btn-block col-md-3">Update</button>
      </router-link>
    </div>
  </main>
</template>

<script>
import firebase from 'firebase'
import db from '@/firebase/init.js'
export default {
  name: 'edit-fundraiser',
  computed:{
    user () {
      return this.$store.state.user
    }
  },
  data () {
    return {
      fname: this.$route.params.name,
      displayname: null,
      description: null,
      city: null,
      stt: null,
      country: null,
      organizer: null,
      restaurant: null,
      userCheck: null
    }
  },
  async created(){
    let finduser = await db.collection('fundraisers').where("uname", "==", this.uname).get()
    this.description = finduser.docs[0].data().description
    this.city = finduser.docs[0].data().city
    this.stt = finduser.docs[0].data().stt
    this.country = finduser.docs[0].data().country
    this.number = finduser.docs[0].data().number
    this.organizer = finduser.docs[0].data().organizer
    this.displayname = finduser.docs[0].data().displayName
  }
}
</script>

<style>
h1{
  color: #444;
}

.section-head{
  padding-left: 8px
}

.content{
  padding-left: 12px
}
</style>
