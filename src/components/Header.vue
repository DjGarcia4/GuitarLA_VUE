<script setup>
import { ref, computed } from "vue";

const props = defineProps({
  cart: {
    type: Array,
    required: true,
  },
  guitar: {
    type: Object,
    required: true,
  },
});

defineEmits(["add-cart", "decrement-product", "increment-product"]);
const cartModal = ref(false);
const totalPay = computed(() => {
  return props.cart.reduce(
    (total, product) => total + product.cant * product.price,
    0
  );
});
</script>
<template>
  <header class="w-full z-50 fixed p-5 flex justify-between">
    <a href="/" class="">
      <img class="w-48" src="/img/logo.svg" alt="imagen logo" />
    </a>
    <button @click="cartModal = !cartModal" class="z-50 flex">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 24 24"
        fill="currentColor"
        class="w-6 h-6 text-white"
      >
        <path
          d="M2.25 2.25a.75.75 0 000 1.5h1.386c.17 0 .318.114.362.278l2.558 9.592a3.752 3.752 0 00-2.806 3.63c0 .414.336.75.75.75h15.75a.75.75 0 000-1.5H5.378A2.25 2.25 0 017.5 15h11.218a.75.75 0 00.674-.421 60.358 60.358 0 002.96-7.228.75.75 0 00-.525-.965A60.864 60.864 0 005.68 4.509l-.232-.867A1.875 1.875 0 003.636 2.25H2.25zM3.75 20.25a1.5 1.5 0 113 0 1.5 1.5 0 01-3 0zM16.5 20.25a1.5 1.5 0 113 0 1.5 1.5 0 01-3 0z"
        />
      </svg>
      <div
        v-if="cart.length > 0"
        class="w-2 h-2 bg-red-500 rounded-full animate-ping text-white"
      ></div>
    </button>
  </header>
  <div
    class="bg-no-repeat bg-cover bg-center p-7"
    style="
      background-image: linear-gradient(
          to right,
          rgba(0, 0, 0, 0.7),
          rgba(0, 0, 0, 0.7)
        ),
        url('../img/header.jpg');
    "
  >
    <div
      v-if="cartModal"
      id="carrito"
      class="bg-white p-3 fixed right-10 top-12 z-50 rounded-lg w-80 md:w-96 shadow-lg"
    >
      <p v-if="cart.length === 0" class="text-center">Cart is empty</p>
      <div v-if="cart.length > 0" class="grid">
        <table class="w-full">
          <thead>
            <tr>
              <th>Image</th>
              <th>Name</th>
              <th>Price</th>
              <th>Cant</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="product in cart">
              <td class="grid place-items-center">
                <img
                  class="img-fluid max-h-20"
                  :src="'/img/' + product.image + '.jpg'"
                  :alt="'image guitar ' + product.name"
                />
              </td>
              <td>{{ product.name }}</td>
              <td class="fw-bold">${{ product.price }}</td>
              <td>
                <div class="grid grid-cols-3 place-items-center">
                  <button
                    type="button"
                    @click="$emit('decrement-product', product.id)"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 24 24"
                      fill="currentColor"
                      class="w-4 h-4 hover:text-red-500 hover:animate-pulse transition-colors"
                    >
                      <path
                        fill-rule="evenodd"
                        d="M5.25 12a.75.75 0 01.75-.75h12a.75.75 0 010 1.5H6a.75.75 0 01-.75-.75z"
                        clip-rule="evenodd"
                      />
                    </svg>
                  </button>
                  {{ product.cant }}
                  <button
                    type="button"
                    @click="$emit('increment-product', product.id)"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 24 24"
                      fill="currentColor"
                      class="w-4 h-4 hover:text-green-500 hover:animate-pulse transition-colors"
                    >
                      <path
                        fillRule="evenodd"
                        d="M12 5.25a.75.75 0 01.75.75v5.25H18a.75.75 0 010 1.5h-5.25V18a.75.75 0 01-1.5 0v-5.25H6a.75.75 0 010-1.5h5.25V6a.75.75 0 01.75-.75z"
                        clipRule="evenodd"
                      />
                    </svg>
                  </button>
                </div>
              </td>
              <td>
                <button type="button">
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 24 24"
                    fill="currentColor"
                    class="w-6 h-6 hover:text-red-500 hover:animate-pulse transition-colors"
                  >
                    <path
                      fill-rule="evenodd"
                      d="M16.5 4.478v.227a48.816 48.816 0 013.878.512.75.75 0 11-.256 1.478l-.209-.035-1.005 13.07a3 3 0 01-2.991 2.77H8.084a3 3 0 01-2.991-2.77L4.087 6.66l-.209.035a.75.75 0 01-.256-1.478A48.567 48.567 0 017.5 4.705v-.227c0-1.564 1.213-2.9 2.816-2.951a52.662 52.662 0 013.369 0c1.603.051 2.815 1.387 2.815 2.951zm-6.136-1.452a51.196 51.196 0 013.273 0C14.39 3.05 15 3.684 15 4.478v.113a49.488 49.488 0 00-6 0v-.113c0-.794.609-1.428 1.364-1.452zm-.355 5.945a.75.75 0 10-1.5.058l.347 9a.75.75 0 101.499-.058l-.346-9zm5.48.058a.75.75 0 10-1.498-.058l-.347 9a.75.75 0 001.5.058l.345-9z"
                      clip-rule="evenodd"
                    />
                  </svg>
                </button>
              </td>
            </tr>
          </tbody>
        </table>

        <p class="text-end font-semibold">
          Total to pay:
          <span class="font-bold text-amber-500 animate-pulse"
            >${{ totalPay }}</span
          >
        </p>
        <button
          class="bg-red-500 m-5 p-1 rounded-md text-white font-bold hover:bg-red-600 transition-colors"
        >
          Empty Cart
        </button>
      </div>
    </div>

    <div class="my-20 md:w-1/2">
      <div class="text-center md:text-left">
        <h1 class="text-amber-500 text-6xl font-bold">
          Model {{ guitar.name }}
        </h1>
        <p class="text-white text-xl mt-14">
          {{ guitar.description }}
        </p>
        <p class="text-amber-500 text-6xl font-extrabold mt-5">
          ${{ guitar.price }}
        </p>
        <button
          type="button"
          class="bg-amber-500 m-5 p-3 rounded-md text-white font-bold hover:bg-amber-600 transition-colors"
          @click="$emit('add-cart', guitar)"
        >
          Add to Cart
        </button>
      </div>
    </div>
    <div>
      <img
        class="header-guitarra h-[610px] xl:h-[583px]"
        src="/img/header_guitarra.png"
        :alt="'image guitar ' + guitar.name"
      />
    </div>
  </div>
</template>

<style scope>
@keyframes animarImagen {
  0% {
    opacity: 0;
    transform: translateX(-10rem);
  }
  50% {
    opacity: 0;
  }
  100% {
    opacity: 1;
    transform: translateX(0);
  }
}
.header-guitarra {
  display: none;
}
@media (min-width: 992px) {
  .header-guitarra {
    display: block;
    position: absolute;
    right: 0;
    top: 0;
    animation-duration: 1s;
    animation-delay: 0s;
    animation-timing-function: ease-in-out;
    animation-name: animarImagen;
  }
}
</style>
