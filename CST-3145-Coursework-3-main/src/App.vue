<template>
  <div id="app" style="margin: 0">
    <title>SmartSchool</title>
    <nav class="navbar navbar-expand-lg text-white navbar-primary bg-dark">
      <div class="container px-4 px-lg-5">
        <h3 class="navbar-logo ms-0 me-4" style="width: 15%"> Smart School</h3>
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarSupportedContent"
          aria-controls="navbarSupportedContent"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0 ms-lg-4">
            <li class="nav-item">
              <a
                class="nav-link-custom nav-link active text-white"
                v-on:click="navigateTo('home')"
                aria-current="page"
                style="cursor: pointer"
                v-if="page === 'home'"
                onmouseover="this.style.outline='1px solid white'; this.style.borderRadius='5px';"
                onmouseout="this.style.outline='none';"
                >Home</a
              >
            </li>
            <li class="nav-item">
              <a
                class="nav-link-custom nav-link text-white"
                v-on:click="navigateTo('home')"
                aria-current="page"
                style="cursor: pointer"
                v-if="page === 'products' || page === 'cart'"
                onmouseover="this.style.outline='1px solid white'; this.style.borderRadius='5px';"
                onmouseout="this.style.outline='none';"
                >Home</a
              >
            </li>
            <li class="nav-item">
              <a
                class="nav-link-custom nav-link active text-white"
                v-on:click="navigateTo('products')"
                style="cursor: pointer"
                v-if="page === 'products'"
                onmouseover="this.style.outline='1px solid white'; this.style.borderRadius='5px';"
                onmouseout="this.style.outline='none';"
                >Lessons</a
              >
            </li>
            <li class="nav-item">
              <a
                class="nav-link-custom nav-link text-white"
                v-on:click="navigateTo('products')"
                style="cursor: pointer"
                v-if="page === 'home' || page === 'cart'"
                onmouseover="this.style.outline='1px solid white'; this.style.borderRadius='5px';"
                onmouseout="this.style.outline='none';"
                >Lessons</a
              >
            </li>
          </ul>
          <form class="d-flex" style="margin-right: 15px">
            <a
              class="btn btn-outline-success"
              type="submit"
              v-on:click="navigateTo('cart')"
            >
              <i class="bi-cart-fill me-1"></i>
              Cart
              <span class="badge bg-dark text-white ms-1 rounded-pill">{{
                cartCount
              }}</span>
            </a>
          </form>
          <form class="d-flex" v-if="cart.length > 0">
            <a
              class="btn btn-outline-success"
              type="submit"
              v-on:click="navigateTo('checkout')"
            >
              <i class="bi-cart-fill me-1"></i>
              Checkout
            </a>
          </form>
        </div>
      </div>
    </nav>

    <section class="py-5 home-section" v-if="page === 'home'">
      <div
        style="
          display: flex;
          align-items: center;
          justify-content: center;
          height: 70vh;
          flex-direction: column;
        "
      >
        <!-- <h1 class="text-white" style="text-align: center">Welcome to Smart School</h1> -->
        <!-- <p class="text-white">
          Lorem ipsum dolor sit amet consectetur adipisicing elit. Quaerat veniam vero tempore autem facere obcaecati tenetur natus mollitia? Porro rerum explicabo iste provident beatae quae dolor, dolores autem magni eligendi?
        </p> -->
        <!-- <h1 class="text-white">Book Now</h1> -->
        <button
          class="btn btn-dark btn-lg"
          type="submit"
          v-on:click="navigateTo('products')"
          style="text-align: center; width: 500px; font-size: 80px; margin-top: 25px"
        >
          Shop now With Us
        </button>
      </div>
    </section>

    <div id="products">
      <ProductList
        :products="this.products"
        @addProduct="addToCart"
        v-if="page === 'products'"
      ></ProductList>
    </div>

    <div id="cart">
      <Cart
        :cart="cart"
        @remove-from-cart="removeFromCart"
        @reduce-quantity="reduceQuantity"
        v-if="page === 'cart'"
      ></Cart>
    </div>

    <div id="checkout">
      <checkOut
        :checkout="checkOut"
        v-if="page === 'checkout'"
      ></checkOut>
    </div>

   
  </div>
</template>

<script>
import ProductList from "./components/productList.vue";
import Cart from "./components/cartList.vue";
import checkOut from "./components/checkOut.vue";
import Swal from "sweetalert2";

export default {
  name: "App",
  components: {
    ProductList,
    Cart, checkOut
  },
  data() {
    return {
      page: "home",
      products: [],
      cart: [],
    };
  },

  methods: {
    navigateTo(page) {
      this.page = page;
    },

    addToCart(product) {
      const existingProduct = this.cart.find((item) => item.id === product.id);
  if (existingProduct) {
    existingProduct.cartquantity++;
  } else {
    const cartProduct = { ...product };
    cartProduct.cartquantity = 1;
    this.cart.push(cartProduct);
  }
  product.cartquantity++;
  product.space--;
      // if (!this.cart.includes(product)) {
      //   this.cart.push(product);
      // } else {
      //   console.log("Product exists in cart");
      // }
      // product.cartquantity++;
      // product.space--;

      // Swal.fire(
      //   "Added to Cart!",
      //   product.subject + " has been added to your cart!",
      //   "success"
      // );
    },
    // async created() {
    //                 let course = await fetch("http://localhost:3000/lesson")
    //                 let result = await course.json()
    //                 this.lessons = result
    //                 console.log(result)
    //             },
    removeFromCart(product) {
      product.cartquantity--;
  product.space--;
  const index = this.cart.findIndex((item) => item.id === product.id);
  if (index !== -1) {
    this.cart.splice(index, 1);
  }
      Swal.fire(
        "Removed from Cart!",
        product.subject + " has been removed from your cart!",
        "success"
      );
    },

    reduceQuantity(product) {
      product.cartquantity--;
      product.space++;
    },

    cartCounter() {
      if (this.cart.length > 0) {
        return true;
      } else {
        return false;
      }
    },
  },

  computed: {
    cartCount() {
      let count = 0;
      this.cart.forEach((item) => {
        count += item.cartquantity;
      });
      console.log(count);
      return count;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.navbar-logo {
  width: 15%;
  margin-right: 1rem;
}

.home-section {
  background-color:thistle;
  background-repeat: no-repeat;
  background-size: cover;
  height: 83.5vh;
}

#products {
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  /* height: 83.5vh; */
}
</style>
