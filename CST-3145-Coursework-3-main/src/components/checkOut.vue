<template>
    <div
        class="container"
        style="margin-top: 50px"
      >
        <main>
          <div class="row g-5">
            <div class="col-md-5 col-lg-4 order-md-last">

            </div>

            <div class="col-md-7 col-lg-8">
              <h1 class="mb-3">Smart School Address</h1>
              <form class="needs-validation" novalidate>
                <div class="row g-3">
                  <div class="col-12">
                    <label for="firstName" class="form-label">Name</label>
                    <input
                      type="text"
                      class="form-control"
                      id="firstName"
                      v-model="order.name"
                      placeholder="Name on ID"
                      required
                    />
                  </div>

                  <div class="col-12">
                    <label for="email" class="form-label">Email</label>
                    <input
                      type="number"
                      class="form-control"
                      id="email"
                      v-model="order.phone"
                      placeholder="you@example.com"
                    />
                  </div>

                </div>
                <hr class="my-4" />

                <button
                  class="w-100 btn btn-primary btn-lg"
                  type="submit"
                  v-on:click="onSubmitCheckout"
                  style="margin-bottom: 25px"
                >
                  Checkout
                </button>
              </form>
            </div>
          </div>
        </main>
      </div>
  </template>

  <script>
  export default {
    name: "checkOut",
    data() {
      return {
        states: {
        AUH: "Abu Dhabi",
        AJM: "Ajman",
        DXB: "Dubai",
        FUJ: "Fujairah",
        RAK: "Ras Al Khaimah",
        SHJ: "Sharjah",
        UMM: "Umm Al Quwain",
      },
      cart: [], // Assuming you have a cart array with items
      order: {
        name: "",
        phone: "",
        address: "",
        city: "",
        zip: "",
        state: "",
        method: "Home",
        gift: false,
      },
      };
    },
    computed: {
      cartTotal() {
  return this.cart.reduce(
    (total, product) => total + product.price * product.space,
    0
  );
},
cartCount() {
  let count = 0;
  this.cart.forEach((item) => {
    count += item.space;
  });
  return count;
},
  },
  methods: {
    navigateTo(page) {
      this.page = page;
    }
  }};
  </script>

  <style>
  /* Add custom styles here */
  </style>
