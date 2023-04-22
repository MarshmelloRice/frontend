<template>
  <div class="card mb-3">
    <div class="row no-gutters">
      <div class="col-md-4">
        <img :src="product.imageURL" :alt="product.name" class="card-img" />
      </div>
      <div class="col-md-8">
        <div class="card-body">
          <h4 class="card-title">{{ product.name }}</h4>
          <h6 class="category font-italic">{{ category.categoryName }}</h6>
          <p class="card-text">{{ product.description }}</p>
          <ul class="list-group list-group-flush">
            <li class="list-group-item">
              <strong>Price:</strong> ${{ product.price }}
            </li>
            <li class="list-group-item">
              <div class="input-group">
                <div class="input-group-prepend">
                  <span class="input-group-text">Quantity:</span>
                </div>
                <input class="form-control" type="number" v-bind:value="quantity" />
              </div>
            </li>
            <li class="list-group-item">
              <button
                type="button"
                id="add-to-cart-button"
                class="btn btn-primary"
                @click="addToCart(this.id)"
              >
                Add to Cart
                <ion-icon name="cart-outline" v-pre></ion-icon>
              </button>
              <button
                id="wishlist-button"
                class="btn btn-light ml-2"
                :class="{ product_added_wishlist: isAddedToWishlist }"
                @click="addToWishList(this.id)"
              >
                {{ wishlistString }}
              </button>
              <button
                id="show-cart-button"
                type="button"
                class="btn btn-light ml-2"
                @click="listCartItems()"
              >
                Show Cart
                <ion-icon name="cart-outline" v-pre></ion-icon>
              </button>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>



<script>
export default {
  data() {
    return {
      product: {},
      category: {},
      id: null,
      token: null,
      isAddedToWishlist: false,
      wishlistString: "Add to wishlist",
      quantity: 1,
    };
  },
  props: ["baseURL", "products", "categories"],
  methods: {
    addToWishList(productId) {
      axios
        .post(`${this.baseURL}wishlist/add?token=${this.token}`, {
          id: productId,
        })
        .then(
          (response) => {
            if (response.status == 201) {
              this.isAddedToWishlist = true;
              this.wishlistString = "Added to WishList";
            }
          },
          (error) => {
            console.log(error);
          }
        );
    },
    addToCart(productId) {
      if (!this.token) {
        swal({
          text: "Please log in first!",
          icon: "error",
        });
        return;
      }
      axios
        .post(`${this.baseURL}cart/add?token=${this.token}`, {
          productId: productId,
          quantity: this.quantity,
        })
        .then(
          (response) => {
            if (response.status == 201) {
              swal({
                text: "Product Added to the cart!",
                icon: "success",
                closeOnClickOutside: false,
              });
              // refresh nav bar
              this.$emit("fetchData");
            }
          },
          (error) => {
            console.log(error);
          }
        );
    },

    listCartItems() {
      axios.get(`${this.baseURL}cart/?token=${this.token}`).then(
        (response) => {
          if (response.status === 200) {
            this.$router.push("/cart");
          }
        },
        (error) => {
          console.log(error);
        }
      );
    },
  },
  mounted() {
    this.id = this.$route.params.id;
    this.product = this.products.find((product) => product.id == this.id);
    this.category = this.categories.find(
      (category) => category.id == this.product.categoryId
    );
    this.token = localStorage.getItem("token");
  },
};
</script>

<style>
.category {
  font-weight: 400;
}

/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type="number"] {
  -moz-appearance: textfield;
}

#add-to-cart-button {
  background-color: #febd69;
}

#wishlist-button {
  background-color: #b9b9b9;
  border-radius: 0;
}

#show-cart-button {
  background-color: #131921;
  color: white;
  border-radius: 0;
}
</style>
