<template>
  <main class="container">
    <div class="jumbotron">
      <h1 align="center">{{fname}}</h1>
      <h4 class="section-head">Description</h4>
      <p class="content">{{description}}</p>
      <h4 class="section-head">Start Date</h4>
      <p class="content">{{startMonth}}</p>
      <p class="content">{{startDay}}</p>
      <p class="content">{{startYear}}</p>
      <h4 class="section-head">End Date</h4>
      <p class="content">{{endMonth}}</p>
      <p class="content">{{endDay}}</p>
      <p class="content">{{endYear}}</p>
      <h4 class="section-head">Fundraising Organizer</h4>
      <p class="content">{{creator}}</p>
      <h4 class="section-head">Restaurant Name</h4>
      <p class="content">{{restaurant}}</p>
      <router-link :to="{ name: 'edit-fundraiser', params: { name: fname }}">
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
      description: null,
      startMonth: null,
      startDay: null,
      startYear: null,
      endMonth: null,
      endDay: null,
      endDayYear: null,
      organizer: null,
      restaurant: null
    }
  },
  async created(){
    let fundraiser = await db.collection('fundraisers').where("fname", "==", this.fname).get()
    this.description = fundraiser.docs[0].data().description
    this.startMonth = fundraiser.docs[0].data().startMonth
    this.startDay = fundraiser.docs[0].data().startDay
    this.startYear = fundraiser.docs[0].data().startYear
    this.endMonth = fundraiser.docs[0].data().endMonth
    this.endDay = fundraiser.docs[0].data().endDay
    this.endYear = fundraiser.docs[0].data().endDayYear
    this.restaurant = fundraiser.docs[0].data().restaurant
    this.organizer = finduser.docs[0].data().creator
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
