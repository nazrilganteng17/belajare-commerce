<template>

  <div>
    <h1 class="mb-10 text-center text-2xl font-bold">Cart Items</h1>
    <div v-if="getCart.length != 0" class="mx-auto max-w-5xl justify-center px-6 md:flex md:space-x-6 xl:px-0">
      <div class="rounded-lg md:w-2/3">
        <div v-for="cart in getCart" :key="cart.cart_id">
        <div class="justify-between mb-6 rounded-lg bg-white p-6 shadow-md sm:flex sm:justify-start">
          <img src="https://image-cdn.hypb.st/https%3A%2F%2Fid.hypebeast.com%2Ffiles%2F2023%2F08%2Ffull-capsule-collection-terbaru-kaws-dan-uniqlo-01.jpg?fit=max&cbr=1&q=90&w=750&h=500" alt="product-image" class="w-full rounded-lg sm:w-40" />
          <div class="sm:ml-4 sm:flex sm:w-full sm:justify-between">
            <div class="mt-5 sm:mt-0">
              <h2 class="text-lg font-bold text-gray-900">{{ cart.name }}</h2>
              <p class="mt-1 text-xs text-gray-700">36EU - 4US</p>
            </div>
            <div class="mt-4 flex justify-between sm:space-y-6 sm:mt-0 sm:block sm:space-x-6">
              <div class="flex items-center border-gray-100">
                <span  @click="changeQty({ cartId: cart.cart_id, typeQty: 'minus'})" class="cursor-pointer rounded-l bg-gray-100 py-1 px-3.5 duration-100 hover:bg-blue-500 hover:text-blue-50"> - </span>
                <input class="h-8 w-8 border bg-white text-center text-xs outline-none" type="" :value="cart.qty" min="1" />
                <span @click="changeQty({ cartId: cart.cart_id, typeQty: 'plus'})" class="cursor-pointer rounded-r bg-gray-100 py-1 px-3 duration-100 hover:bg-blue-500 hover:text-blue-50"> + </span>
              </div>
              <div class="flex items-center space-x-4">
                <p class="text-sm">Rp. {{ cart.regular_price }}</p> 
                <button type="button" @click="removeItem(cart.cart_id)">
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="h-5 w-5 cursor-pointer duration-150 hover:text-red-500">
                  <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
                </svg>
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
      </div>
      <!-- Sub total -->
      <div class="mt-6 h-full rounded-lg border bg-white p-6 shadow-md md:mt-0 md:w-1/3">
        <div class="mb-2 flex justify-between">
          <p class="text-gray-700">Subtotal</p>
          <p class="text-gray-700">{{parseInt (totalHarga)}}</p>
        </div>
        <hr class="my-4" />
        <div class="flex justify-between">
          <p class="text-lg font-bold">Total</p>
          <div class="">
            <p class="mb-1 text-lg font-bold">{{ parseInt  (totalHarga) }}</p>
            <p class="text-sm text-gray-700">including VAT</p>
          </div>
        </div>
        <router-link to="/checkout">
        <button class="mt-6 w-full rounded-md bg-blue-500 py-1.5 font-medium text-blue-50 hover:bg-blue-600">Check out</button>
      </router-link>
      </div>
    </div>
    <div v-else>
      <p>Data tidak ditemukan</p>
    </div>
  
  </div><br><br><br>

</template>
<script>
import { mapGetters, mapActions } from 'vuex';

export default {
    computed: {
        ...mapGetters('cart', ['getCart']),
        totalHarga(){
          this.total = this.getCart.reduce((acc,product) => {
            return acc + parseFloat(product.regular_price * product.qty);
          },0);
          return this.total.toFixed(2);
        },
    },
    methods: {
        ...mapActions('cart', ['fetchCart']),
        ...mapActions('product',['fetchProduk']),
        

        changeQty(cartId, typeQty) {
          this.$store.dispatch('cart/changeQuantityCart', cartId, typeQty)
        },
        // REMOVE PRODUCT FROM CART
        removeItem(cartId) {
          this.$store.dispatch('cart/removeFromCart', cartId)
        }
    },
    beforeMount(){
      this.fetchProduk();
    },
    mounted() {
        this.fetchCart();
    }
}
</script>

<!-- <template>
    <section class="h-screen bg-gray-100 py-12 sm:py-16 lg:py-20">
  <div class="mx-auto px-4 sm:px-6 lg:px-8">
    <div class="flex items-center justify-center">
      <h1 class="text-2xl font-semibold text-gray-900">Your Cart</h1>
    </div>

    <div class="mx-auto mt-8 max-w-2xl md:mt-12">
      <div class="bg-white shadow">
        <div class="px-4 py-6 sm:px-8 sm:py-10">
          <div class="flow-root">
            <ul class="-my-8">
              <li class="flex flex-col space-y-3 py-6 text-left sm:flex-row sm:space-x-5 sm:space-y-0">
                <div class="shrink-0">
                  <img class="h-24 w-24 max-w-full rounded-lg object-cover" src="https://sneakers.id/images/products/553558-040/2_1672814156.jpg" alt="" />
                </div>

                <div class="relative flex flex-1 flex-col justify-between">
                  <div class="sm:col-gap-5 sm:grid sm:grid-cols-2">
                    <div class="pr-8 sm:pr-5">
                      <p class="text-base font-semibold text-gray-900">AIR JORDAN 1 LOW SHADOW 3.0  </p>
                      <p class="mx-0 mt-1 mb-0 text-sm text-gray-400">36EU - 4US</p>
                    </div>

                    <div class="mt-4 flex items-end justify-between sm:mt-0 sm:items-start sm:justify-end">
                      <p class="shrink-0 w-20 text-base font-semibold text-gray-900 sm:order-2 sm:ml-8 sm:text-right">3.899.000</p>

                      <div class="sm:order-1">
                        <div class="mx-auto flex h-8 items-stretch text-gray-600">
                          <button class="flex items-center justify-center rounded-l-md bg-gray-200 px-4 transition hover:bg-black hover:text-white">-</button>
                          <div class="flex w-full items-center justify-center bg-gray-100 px-4 text-xs uppercase transition">1</div>
                          <button class="flex items-center justify-center rounded-r-md bg-gray-200 px-4 transition hover:bg-black hover:text-white">+</button>
                        </div>
                      </div>
                    </div>
                  </div>

                  <div class="absolute top-0 right-0 flex sm:bottom-0 sm:top-auto">
                    <button type="button" class="flex rounded p-2 text-center text-gray-500 transition-all duration-200 ease-in-out focus:shadow hover:text-gray-900">
                      <svg class="h-5 w-5" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" class=""></path>
                      </svg>
                    </button>
                  </div>
                </div>
              </li>
            </ul>
          </div>

          <div class="mt-6 border-t border-b py-2">
            <div class="flex items-center justify-between">
              <p class="text-sm text-gray-400">Subtotal</p>
              <p class="text-lg font-semibold text-gray-900">3.899.000</p>
            </div>
            <div class="flex items-center justify-between">
              <p class="text-sm text-gray-400">Disc</p>
              <p class="text-lg font-semibold text-gray-900">12% </p>
            </div>
          </div>
          <div class="mt-6 flex items-center justify-between">
            <p class="text-sm font-medium text-gray-900">Total</p>
            <p class="text-2xl font-semibold text-gray-900"><span class="text-xs font-normal text-gray-400">Rp</span> 3.399.000</p>
          </div>

          <div class="mt-6 text-center">
            <a href="/checkout">
                <button type="button" class="group inline-flex w-full items-center justify-center rounded-md bg-gray-900 px-6 py-4 text-lg font-semibold text-white transition-all duration-200 ease-in-out focus:shadow hover:bg-gray-800">
              Checkout
              <svg xmlns="http://www.w3.org/2000/svg" class="group-hover:ml-8 ml-4 h-6 w-6 transition-all" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                <path stroke-linecap="round" stroke-linejoin="round" d="M13 7l5 5m0 0l-5 5m5-5H6" />
              </svg>
            </button>
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>
<br>
<br>
<br>
<br>
</template> -->