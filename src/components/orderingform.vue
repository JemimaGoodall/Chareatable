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
            <button class="btn btn-primary btn-lg btn-block col-md-3" type="submit" @click="EDITTHIS!!!()">Order</button>
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
    async checkAvailability () {
      let getcode = await db.collection('fundraisers').where("restaurant", "==", this.restaurant).get()
      if (this.restaurant == null || this.uname == "") {
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
      items: null,
      method: null,
      payment: null,
      restaurant: this.$route.params.rname,
      comment: null,
      address: null,
      recipient: null,
      card: null
    }
  },
  mounted: function(){
    this.checkAvailability()
  },
  async created(){
    let fundraiser = await db.collection('fundraisers').where("fname" == "this.fname").get()
    this.items = items.d
    this.comment = fundraiser.comment

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
