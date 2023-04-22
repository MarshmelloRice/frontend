<template>
  <div class="container-fluid">
    <div class="row justify-content-center">
      <div class="col-md-8">
        <h3 class="text-center pt-3">Shopping Cart</h3>
        <!--    loop over all the cart items and display one by one-->
        <div
          v-for="cartItem in cartItems"
          :key="cartItem.product.id"
          class="card mb-3"
        >
          <div class="row no-gutters">
            <!-- display image -->
            <div class="col-md-3">
              <router-link
                :to="{ name: 'ShowDetails', params: { id: cartItem.product.id } }"
              >
                <img
                  v-bind:src="cartItem.product.imageURL"
                  class="card-img"
                  alt="Product Image"
                />
              </router-link>
            </div>
            <!-- display product name, quantity and price -->
            <div class="col-md-9">
              <div class="card-body">
                <h5 class="card-title">
                  <router-link
                    :to="{ name: 'ShowDetails', params: { id: cartItem.product.id } }"
                  >
                    {{ cartItem.product.name }}
                  </router-link>
                </h5>
                <p class="card-text">
                  <span class="font-weight-bold">Price per unit:</span>
                  $ {{ cartItem.product.price }}
                </p>
                <div class="form-group">
                  <label for="quantity">Quantity:</label>
                  <input
                    type="number"
                    class="form-control w-25 d-inline-block"
                    id="quantity"
                    v-model="cartItem.quantity"
                    min="1"
                    max="100"
                    required
                  />
                </div>
                <p class="card-text">
                  <span class="font-weight-bold">Total Price:</span>
                  $ {{ cartItem.product.price * cartItem.quantity }}
                </p>
                <button
                  type="button"
                  class="btn btn-sm btn-danger float-right"
                  @click="deleteItem(cartItem.id)"
                >
                  Remove From Cart
                </button>
              </div>
            </div>
          </div>
        </div>

        <!-- display total price -->
        <div class="total-cost pt-2 text-right">
          <h5>Total: $ {{ totalcost.toFixed(2) }}</h5>
          <button
            :disabled="isDisabled()"
            type="button"
            class="btn btn-primary confirm"
            @click="checkout"
          >
            Confirm Order
          </button>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
const axios = require('axios');
export default {
  data() {
    return {
      cartItems: [],
      token: null,
      totalcost: 0,
    };
  },
  name: 'Cart',
  props: ['baseURL'],
  methods: {
    isDisabled() {
      if (this.cartItems.length === 0) {
        return true;
      }
      return false;
    },
    // fetch all the items in cart
    listCartItems() {
      axios.get(`${this.baseURL}cart/?token=${this.token}`).then(
        (response) => {
          if (response.status == 200) {
            const result = response.data;
            // store cartitems and total price in two variables
            this.cartItems = result.cartItems;
            this.totalcost = result.totalCost;
          }
        },
        (error) => {
          console.log(error);
        }
      );
    },
    // go to checkout page
    checkout() {
      this.$router.push({ name: 'Checkout' });
    },
    deleteItem(itemId) {
      axios
        .delete(`${this.baseURL}cart/delete/${itemId}/?token=${this.token} `)
        .then(
          (response) => {
            if (response.status == 200) {
              this.$router.go(0);
            }
            this.$emit('fetchData');
          },
          (error) => {
            console.log(error);
          }
        );
    },
    showDetails(productId) {
      this.$router.push({
        name: 'ShowDetails',
        params: { id: productId },
      });
    },
  },
  mounted() {
    this.token = localStorage.getItem('token');
    this.listCartItems();
  },
};
</script>

<style scoped>
h4,
h5 {
  font-family: 'Roboto', sans-serif;
  color: #484848;
  font-weight: 700;
}

.embed-responsive .card-img-top {
  object-fit: cover;
}

#item-price {
  color: #232f3e;
}

#item-quantity {
  float: left;
}

#item-total-price {
  float: right;
}

.confirm {
  font-weight: bold;
  font-size: larger;
}
</style>
