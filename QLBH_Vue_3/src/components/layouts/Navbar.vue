<template lang="">
  <div class="menu">
    <nav class="bg-white border-gray-200 dark:bg-gray-900">
      <div class="flex flex-wrap items-center justify-between mx-auto py-3 px-16">
        <router-link
          to="/"
          class="flex items-center space-x-3 rtl:space-x-reverse"
        >
          <img
            :src="require('@/assets/images/logo.png')"
            class="h-16"
            alt="Flowbite Logo"
          />
        </router-link>
        <button
          data-collapse-toggle="navbar-default"
          type="button"
          class="inline-flex items-center p-2 w-10 h-10 justify-center text-sm text-gray-500 rounded-lg md:hidden hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-gray-200 dark:text-gray-400 dark:hover:bg-gray-700 dark:focus:ring-gray-600"
          aria-controls="navbar-default"
          aria-expanded="false"
        >
          <span class="sr-only">Open main menu</span>
          <svg
            class="w-5 h-5"
            aria-hidden="true"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 17 14"
          >
            <path
              stroke="currentColor"
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M1 1h15M1 7h15M1 13h15"
            />
          </svg>
        </button>
        <div class="hidden w-full md:block md:w-auto">
          <ul
            class="font-medium flex items-center flex-col p-4 md:p-0 mt-4 border border-gray-100 rounded-lg bg-gray-50 md:flex-row md:space-x-8 rtl:space-x-reverse md:mt-0 md:border-0 md:bg-white dark:bg-gray-800 md:dark:bg-gray-900 dark:border-gray-700"
          >
            <li>
              <button @click="showCartLeft">
                <div class="relative py-2 px-2">
                  <div class="top-3 absolute left-5">
                    <p
                      class="flex h-2 w-2 items-center justify-center rounded-full bg-red-500 p-3 text-xs text-white"
                    >
                      {{ cartCount }}
                    </p>
                  </div>
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke-width="1.5"
                    stroke="currentColor"
                    class="file: mt-4 h-6 w-6"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      d="M2.25 3h1.386c.51 0 .955.343 1.087.835l.383 1.437M7.5 14.25a3 3 0 00-3 3h15.75m-12.75-3h11.218c1.121-2.3 2.1-4.684 2.924-7.138a60.114 60.114 0 00-16.536-1.84M7.5 14.25L5.106 5.272M6 20.25a.75.75 0 11-1.5 0 .75.75 0 011.5 0zm12.75 0a.75.75 0 11-1.5 0 .75.75 0 011.5 0z"
                    />
                  </svg>
                </div>
              </button>
            </li>
            <li class="li-parent-dropdown" v-if="$store.state.user !== null && $store.state.user !== undefined">
              <button id="dropdownDefaultButton" @click="showDropdownClick" data-dropdown-toggle="dropdown" class="inline-flex items-center py-2 px-3 text-gray-900 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-white md:dark:hover:text-blue-500 dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent" type="button">
                <span class="mr-2">Account</span> 
                <svg class="w-2.5 h-2.5 ms-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 10 6">
                  <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m1 1 4 4 4-4"/>
                </svg>
              </button>
              <!-- Dropdown menu -->
              <div id="dropdown" :class="{ hidden: !showDropdown }" class="z-10 ul-dropdown-absolute bg-white divide-y divide-gray-100 rounded-lg shadow w-44 dark:bg-gray-700">
                  <ul class="py-2 text-sm text-gray-700 dark:text-gray-200" aria-labelledby="dropdownDefaultButton">
                    <li>
                      <router-link to="/profile" class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">Profile</router-link>
                    </li>
                    <li>
                      <router-link to="/list-order" class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">List Order</router-link>
                    </li>
                    <li v-if="role === 'admin'">
                      <router-link
                        to="/list-check-order"
                        class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white"
                        >Check Order</router-link>
                    </li>
                    <li v-if="role === 'admin'">
                      <router-link
                        to="/list-product"
                        class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white"
                        >List Product</router-link
                      >
                    </li>
                    <li v-if="role === 'admin'">
                      <router-link
                        to="/add-product"
                        class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white"
                        >Add Product</router-link
                      >
                    </li>
                  </ul>
              </div>
            </li>
            <li v-if="$store.state.user !== null && $store.state.user !== undefined">
              <button
                @click="$store.dispatch('logout')"
                to="/login"
                class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-blue-600 text-base font-medium text-white hover:bg-blue-700 hover:text-white focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 sm:ml-3 sm:w-auto sm:text-sm"
              >
                Logout
              </button>
            </li>
            <li v-if="!$store.state.user">
              <router-link
                to="/login"
                class="w-full inline-flex justify-center rounded-md border border-transparent shadow-sm px-4 py-2 bg-blue-600 text-base font-medium text-white hover:bg-blue-700 hover:text-white focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 sm:ml-3 sm:w-auto sm:text-sm"
                >Login</router-link
              >
            </li>
          </ul>
        </div>
      </div>
    </nav>
  </div>
  <CartLeft :show="showCart" @close="showCart = false" />
</template>

<script>
import { useStore } from "vuex";
import { onBeforeMount, onMounted,computed } from "vue";
import CartService from "@/services/CartService";
import AuthService from "@/services/AuthService";

import CartLeft from "./CartLeft.vue";

export default {
  components: { CartLeft },
  data() {
    return {
      showCart: false,
      showDropdown: false,
    };
  },
  methods: {
    showCartLeft() {
      this.showCart = !this.showCart;
    },
    showDropdownClick() {
      this.showDropdown = !this.showDropdown;
    },
  },
  setup() {
    const store = useStore();

    const cartCount = computed(() => store.state.cartCount);

    const carts = computed(() => CartService.getCart());

    const role = computed(() => store.state.user.role || AuthService.getCurentUser().role);

    onBeforeMount(() => {
      store.dispatch("fetchUser");
    });

    onMounted(() => {
      cartCount.value = CartService.getCart().length;
      console.log(store.state.user.role);
    })

    return {
      user: store.state.user,
      cartCount,
      carts,
      role
    };
  },
};
</script>
<style lang="css">
.li-parent-dropdown {
  position: relative;
}
.ul-dropdown-absolute {
  position: absolute;
  top: calc(100% + 10px);
  width: 170px;
  left: -10%;
  background: #fff;
  box-shadow: 0 0 4px 1px #efefef;
  border-radius: 5px;
}
</style>
