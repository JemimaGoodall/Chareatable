<template>
  <div class="container">
            <h4 class="mb-3">Order Food</h4>
            <form v-if="user">
              <div class="row">
                <div class="col-md-auto mb-3">
                  <label for="items">items</label>
                  <div class="input-group">
                    <div class="input-group-prepend">
                      <!--<span class="input-group-text">@</span>
                    </div>
                    <input type="text" class="form-control" id="username" v-model="fname" @input="checkAvailability()">
                    <div class="availability">
                      <i v-if="fnameempty" class="material-icons red">close</i>
                      <i v-else-if="available" class="material-icons green">check</i>
                      <i v-else-if="unavailable" class="material-icons red">close</i>
                    </div>
                  </div>
                  <p v-if="fnameempty" class="red availability">Enter a Username</p>
                  <p v-else-if="available" class="green availability">Name available!</p>
                  <p v-else-if="unavailable" class="red availability">Name unavailable!</p>
                </div> -->
              </div>
              <div class="mb-3">
                <label for="method">method</label>
                <select name ="delivery" size = "2">
                    <option value = "pick-up">pick-up</option>
                    <option value = "delivery">delivery</option>
                  </select>
                  <label for="payment">payment</label>
                  <select name ="payment" size = "2">
                      <option value = "cash">card</option>
                      <option value = "card">card</option>
                    </select>
                <!--<textarea class="form-control" rows="5" id="bio" v-model="method"></textarea>-->
              </div>
              <div class="row">
                <div class="col-md-4 mb-3">
                  <label for="comment">Comment</label>
                  <input type="text" class="form-control" id="city" v-model="comment">
                </div>
                <div class="col-md-4 mb-3">
                  <label for="card">Card number</label>
                  <input type="text" class="form-control" id="state" v-model="card">
                </div>
                <div class="col-md-4 mb-3">
                  <label for="address">address</label>
                  <input type="text" class="form-control" id="country" v-model="address">
                </div>
              </div>
              <div class="row">
                <div class="col-md-4 mb-3">
                  <label for="recipient">recipient</label>
                  <input type="text" class="form-control" id="city" v-model="recipient">
                </div>
              </div>


              <hr class="mb-4">
            </form>
            <button :disabled="unavailable||unameempty" class="btn btn-primary btn-lg btn-block col-md-3" type="submit" @click="updateProfile()">Update</button>
          </div>
</template>

<script>
import firebase from 'firebase'
import db from '@/firebase/init.js'
export default {
  name: 'order',
  computed:{
    user () {
      return this.$store.state.fundraisers
    }
  },
  methods: {
    async updateProfile () {
      const ref = db.collection('fundraisers')
      await ref.add({
        description: this.description,
        startMonth: this.startMonth,
        startDay: this.startDay,
        startYear: this.startYear,
        endMonth: this.endMonth,
        endDay: this.endDay,
        endYear: this.endYear,
        restaurant: this.restaurant,
        fname: this.fname,
        creator: this.user.uname
      })
      this.$router.push({ name: "fundraiser", params: { fname: this.fname }})
    },
    async checkAvailability () {
      let checkname = await db.collection('fundraisers').where("fname", "==", this.fname).get()
      if (this.fname == null || this.uname == "") {
        this.fnameempty = true
      } else if (checkname.empty || checkname.docs[0].data().fname == this.fname) {
        this.available = true
        this.fnameempty = false
        this.unavailable = false
      }
      else {
        this.available = false
        this.fnameempty = false
        this.unavailable = true
      }
    }
  },
  data () {
    return {
      description: null,
      startMonth: null,
      startDay: null,
      startYear: null,
      restaurant: null,
      fname: null,
      available: null,
      unavailable: null,
      fnameempty: null,
      creator: null
    }
  },
  mounted: function(){
    this.checkAvailability()
  },
  async created(){
    let fundraiser = await db.collection('fundraisers').where("fname" == "this.fname").get()
    this.description = fundraiser.description
    this.startMonth = fundraiser.startMonth
    this.startDay = fundraiser.startDay
    this.startYear = fundraiser.startYear
    this.endMonth = fundraiser.endMonth
    this.endDay = fundraiser.endDay
    this.endYear = fundraiser.endYear
    this.restaurant = fundraiser.restaurant
    this.fname = fundraiser.fname
    this.creator = fundraiser.creator
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
