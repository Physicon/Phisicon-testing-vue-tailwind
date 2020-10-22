<template>
  <div class="item__wrapper w-full sm:w-1/2 lg:w-1/3 xl:w-1/4 my-2">
    <div class="item px-5 py-4 mx-2 h-32 border border-gray-600 rounded-lg shadow-md">
      <strong class="block truncate" :title="item.title">{{ item.title }}</strong>
      <span class="block mt-3">{{ item.grade }} класс</span>
      <button v-if="payment" class="block w-full text-white bg-blue-500 rounded-lg px-4 py-1 mt-2">{{ item.price }} руб.</button>
      <button v-else class="block w-full text-white bg-blue-500 rounded-lg px-4 py-1 mt-2">{{ item.priceBonus }} бонусов</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Item",
  props: {
    item: {
      type: Object,
      required: true
    },
    payment: {
      type: Boolean,
      required: true
    }
  }
}
</script>

<style scoped>

</style>
