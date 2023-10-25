<script setup>
const props = defineProps({
  guitar: {
    type: Object,
    required: true,
  },
});

defineEmits(["add-cart"]);
</script>
<template>
  <div class="flex shadow-md rounded-md items-center bg-white p-4">
    <div class="">
      <img
        class="img-fluid"
        :src="'/img/' + guitar.image + '.jpg'"
        :alt="'image guitar ' + guitar.name"
      />
    </div>
    <div class="space-y-2">
      <h3 class="font-bold uppercase text-xl">{{ guitar.name }}</h3>
      <p class="text-sm">
        {{ guitar.description }}
      </p>
      <p class="text-amber-500 text-3xl font-bold">${{ guitar.price }}</p>
      <button
        @click="$emit('add-cart', guitar)"
        type="button"
        class="bg-amber-500 p-2 rounded-md text-white font-bold hover:bg-amber-600 transition-colors"
      >
        <v-tooltip activator="parent" location="right">Add Product</v-tooltip>
        Add to Cart
      </button>
    </div>
  </div>
</template>
