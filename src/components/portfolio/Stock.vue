<template lang="html">
  <div class="col-sm-6 col-md-4">
    <div class="panel panel-info">
      <div class="panel-heading">
        <h3 class="panel-title">
          {{ stock.name }}
          <small>(Price: {{ stock.price }} | {{ stock.quantity }})</small>
        </h3>
      </div>
      <div class="panel-body">
        <div>
          <input type="number"
                 class="form-control"
                 placeholder="Quantity"
                 v-model="quantity"
                 :class="{danger: insufficientQuantity}">
        </div>
        <br>
        <div>
          <button class="btn btn-success"
                  @click="sellStocks"
                  :disabled="insufficientQuantity || quantity <= 0">
          {{ insufficientQuantity ? 'Not enough Stocks' : 'Sell'}}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapActions } from 'vuex'
export default {
  props: ['stock'],
  data () {
    return {
      quantity: 0
    }
  },
  computed: {
    insufficientQuantity () {
      return this.quantity > this.stock.quantity
    }
  },
  methods: {
    ...mapActions([
      'sellStock'
    ]
    ),
    sellStocks () {
      if (this.quantity % 1 == 0) {
        const order = {
          stockId: this.stock.id,
          stockPrice: this.stock.price,
          quantity: this.quantity
        }
        this.sellStock(order)
        this.quantity = 0
      }
    }
  }
}
</script>

<style lang="css" scoped>
  .danger {
    border: 1px solid red;
  }
</style>
