<template>
  <div class="container">
            <h4 class="mb-3">Edit Fundraiser</h4>
            <form v-if="user">
              <div class="row">
                <div class="col-md-auto mb-3">
                  <label for="username">Fundraiser name</label>
                  <div class="input-group">
                    <div class="input-group-prepend">
                      <span class="input-group-text">@</span>
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
                </div>
              </div>
              <div class="mb-3">
                <label for="bio">Fundraising description</label>
                <textarea class="form-control" rows="5" id="bio" v-model="description"></textarea>
              </div>
              <div class="row">
                <div class="col-md-4 mb-3">
                  <label for="city">Start Date</label>
                  <input type="text" class="form-control" id="city" v-model="startMonth" placeholder="mm">
                </div>
                <div class="col-md-4 mb-3">
                  <label for="state"></label>
                  <input type="text" class="form-control" id="state" v-model="startDay" placeholder="dd">
                </div>
                <div class="col-md-4 mb-3">
                  <label for="country"></label>
                  <input type="text" class="form-control" id="country" v-model="startYear" placeholder="yy">
                </div>
              </div>
              <div class="row">
                <div class="col-md-4 mb-3">
                  <label for="city">End Date</label>
                  <input type="text" class="form-control" id="city" v-model="endMonth" placeholder="mm">
                </div>
                <div class="col-md-4 mb-3">
                  <label for="state"></label>
                  <input type="text" class="form-control" id="state" v-model="endDay" placeholder="dd">
                </div>
                <div class="col-md-4 mb-3">
                  <label for="country"></label>
                  <input type="text" class="form-control" id="country" v-model="endYear" placeholder="yy">
                </div>
              </div>
              <div class="row">
                <div class="col-md-4 mb-3">
                  <label for="number">Restaurant</label>
                  <input type="text" class="form-control" id="number" v-model="restaurant">
                </div>
                <div class="col-md-4 mb-3">
                  <label for="code">Coupon code</label>
                  <input type="text" class="form-control" id="country" v-model="code">
                </div>

              </div>


              <hr class="mb-4">
            </form>
            <button :disabled="unavailable||fnameempty" class="btn btn-primary btn-lg btn-block col-md-3" type="submit" @click="updateProfile()">Update</button>
          </div>
</template>

<script>
import firebase from 'firebase'
import db from '@/firebase/init.js'
export default {
  name: 'editfundraiser',
  computed:{
    user () {
      return this.$store.state.user
    }
  },
  methods: {
    async updateProfile () {

      const ref = await db.collection('fundraisers').where('fname', "==", this.fname).get()
      const reference = db.collection('fundraisers').doc(ref.docs[0].id)
      await reference.update({
        description: this.description,
        startMonth: this.startMonth,
        startDay: this.startDay,
        startYear: this.startYear,
        endMonth: this.endMonth,
        endDay: this.endDay,
        endYear: this.endYear,
        restaurant: this.restaurant,
        code: this.code,
        fname: this.fname,

      })
      this.$router.push({ name: "fundraiser", params: { name: this.fname }})
    },
    async checkAvailability () {

      let checkname = await db.collection('fundraisers').where("fname", "==", this.fname).get()
      if (this.fname == null || this.fname == "") {
        this.fnameempty = true
      } else if (checkname.empty || this.fname == checkname.docs[0].data().fname) {

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
      endMonth: null,
      endDay: null,
      endYear: null,
      restaurant: null,
      code: nul,
      fname: this.$route.params.name,
      available: null,
      unavailable: null,
      fnameempty: null,

    }
  },
  mounted: function(){
    this.checkAvailability()
  },
  async created(){
    let fundraiser = await db.collection('fundraisers').where("fname", "==", this.fname).get()
    this.description = fundraiser.docs[0].data().description
    this.startMonth = fundraiser.docs[0].data().startMonth
    this.startDay = fundraiser.docs[0].data().startDay
    this.startYear = fundraiser.docs[0].data().startYear
    this.endMonth = fundraiser.docs[0].data().endMonth
    this.endDay = fundraiser.docs[0].data().endDay
    this.endYear = fundraiser.docs[0].data().endYear
    this.restaurant = fundraiser.docs[0].data().restaurant
    this.code = fundraiser.docs[0].data().code

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
