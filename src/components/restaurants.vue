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
  name: 'restaurants',
  computed:{
    user () {
      return this.$store.state.user
    }
  },
  methods: {

    async checkAvailability () {
      let checkname = await db.collection('fundraisers').where("blah", "==", this.uname).get()
      if (this.uname == null || this.uname == "") {
        this.unameempty = true
      } else if (checkname.empty || checkname.docs[0].data().uname == this.user.uname) {
        this.available = true
        this.unameempty = false
        this.unavailable = false
      }
      else {
        this.available = false
        this.unameempty = false
        this.unavailable = true
      }
    }
  },
  data () {
    return {
      fname: this.$route.params.name,
      bio: null,
      city: null,
      stt: null,
      country: null,
      number: null,
      affiliation: null,
      uname: null,
      available: null,
      unavailable: null,
      unameempty: null,
      id: null,
    }
  },
  mounted: function(){
    this.checkAvailability()
  },
  async created(){
    let fundraiser = await db.collection('fundraisers').where("name", "==", this.fname).get()
    this.id = fundraiser.docs[0].id
    this.bio = this.user.bio
    this.city = fundraiser.docs[0].data().city
    this.stt = this.user.stt
    this.country = this.user.country
    this.number = this.user.number
    this.affiliation = this.user.affiliation
  }
}
</script>

<style>
.container{
  padding-top: 40px;
  padding-bottom: 40px;
}
.material-icons.green { color: green;
}

.material-icons.red { color: red;
}

.availability{
  padding-top: 6px;
  padding-left: 3px;
}

.green{
  color: green;
}

.red{
  color: red;
}

</style>
