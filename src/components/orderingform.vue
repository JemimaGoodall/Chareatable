<template>
  <div class="container">
            <h4 class="mb-3">Order Food</h4>
            <form>
              <div class="row">
                <div class="col-md-4 mb-3">
                  <label for="comment">Coupon Code</label>
                  <input type="text" class="form-control" id="comment" v-model="comment">
                </div>
              </div>
              <hr class="mb-4">
            </form>
            <button class="btn btn-primary btn-lg btn-block col-md-3" type="submit" @click="orderFood()">Pay</button>
          </div>
</template>

<script>
import firebase from 'firebase'
import db from '@/firebase/init.js'
import request from 'request'
export default {
  name: 'order',
  computed:{
    user () {
      return this.$store.state.user
    }
  },
  methods: {
    orderFood: function() {
      var options = { method: 'POST',
        url: 'https://eatstreet.com/publicapi/v1/send-order',
        headers:
         { 'X-Access-Token': '41b80af99d9b4d26',
           'Content-Type': 'application/json' },
        body:
         { restaurantApiKey: '626ff59d94b09e9af15b4f2e73e76180ceaf86853e1bc0c4',
           items: [ { apiKey: '11947703', customizationChoices: [], comments: '' } ],
           method: 'pickup',
           payment: 'cash',
           test: true,
           comment: this.comment,
           card: { apiKey: null },
           address: { apiKey: null },
           recipient:
            { firstName: 'Rishab',
              lastName: 'Nayak',
              email: '12345@mailinator.com',
              phone: '2345671121' } },
        json: true };

      request(options, function (error, response, body) {
      });
      this.$router.push({ name: "success"})
    },
  },
  data () {
    return {
      restaurant: this.$route.params.restname,
      comment: null,
    }
  },
  async created(){
    let fundraiser = await db.collection('fundraisers').where("restaurant", "==", this.restaurant).get()
    this.comment = fundraiser.docs[0].data().code
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
