<template>
  <div class="product container">
    <Product v-for="(product, i) in productArr" :key="i" :productData="product" />
  </div>
</template>

<script setup>
import Product from './Product.vue';
import { useProduct } from "@/store/index.js";
import { computed, watchEffect, ref, onMounted, watch } from "vue"; 

const emit = defineEmits(['limitChanged'])
const productStore = useProduct()
const productArr = computed(() => productStore.arr)
const isActiveRef = ref(false)
const isActive = computed(() => productStore.isFilterActive)
isActiveRef.value = isActive.value
const limit = ref(100)
productStore.getProducts()

const props = defineProps({
  filterValue: {
    type: String,
    required: true
  }
})


watchEffect(() => {
  productStore.sortGoods(props.filterValue)
});

watch(limit, () => {
  emit('limitChanged', limit.value)
})
</script>

<style lang="scss" scoped></style>
