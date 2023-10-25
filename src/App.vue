<script setup>
import { ref, watch, onMounted } from "vue";
import { db } from "./data/guitars";
import { useToast } from "vue-toast-notification";
import "vue-toast-notification/dist/theme-sugar.css";

import Guitar from "./components/Guitar.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";

const $toast = useToast();

const guitars = ref([]);
const cart = ref([]);
const guitar = ref({});

watch(
  cart,
  () => {
    saveLocalStorage();
  },
  { deep: true }
);

onMounted(() => {
  guitars.value = db;
  guitar.value = db[3];
  const cartStorage = localStorage.getItem("cart");
  if (cartStorage) {
    cart.value = JSON.parse(cartStorage);
  }
});

const saveLocalStorage = () => {
  localStorage.setItem("cart", JSON.stringify(cart.value));
};

const addCart = (guitar) => {
  const existCart = cart.value.findIndex((product) => product.id === guitar.id);
  if (existCart >= 0) {
    if (cart.value[existCart].cant === 10) {
      $toast.error(`Invalid cant!`);
      return;
    }
    cart.value[existCart].cant++;
  } else {
    guitar.cant = 1;
    cart.value.push(guitar);
    $toast.success(`${guitar.name} added to the cart!`);
  }
  saveLocalStorage();
};

const decrementProduct = (id) => {
  const existCart = cart.value.findIndex((product) => product.id === id);
  if (existCart >= 0) {
    if (cart.value[existCart].cant === 1) {
      $toast.error(`Invalid cant!`);
      return;
    }
    cart.value[existCart].cant--;
    return;
  }
};
const incrementProduct = (id) => {
  const existCart = cart.value.findIndex((product) => product.id === id);
  if (existCart >= 0) {
    if (cart.value[existCart].cant === 10) {
      $toast.error(`Invalid cant!`);
      return;
    }
    cart.value[existCart].cant++;
    return;
  }
};

const deleteProduct = (id) => {
  cart.value = cart.value.filter((product) => product.id !== id);
  $toast.success(`Product Deleted`);
};
const emptyCart = () => {
  cart.value = [];
  $toast.success(`Cart Emptied`);
};
</script>

<template>
  <Header
    :cart="cart"
    :guitar="guitar"
    @add-cart="addCart"
    @decrement-product="decrementProduct"
    @increment-product="incrementProduct"
    @delete-product="deleteProduct"
    @empty-cart="emptyCart"
  />

  <main class="m-10">
    <h2 class="text-amber-500 text-4xl font-extrabold text-center mb-4">
      Our Collection
    </h2>

    <div
      class="grid place-items-center grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-3"
    >
      <Guitar v-for="guitar in guitars" :guitar="guitar" @add-cart="addCart" />
    </div>
  </main>
  <Footer />
</template>
