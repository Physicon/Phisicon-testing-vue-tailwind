<template>
  <div class="item__wrapper w-full sm:w-1/2 lg:w-1/3 xl:w-1/5 mt-16">
    <div class="item mx-4 border border-gray-500 rounded-lg shadow-md overflow-hidden">
      <div class="image__wrapper">
        <img class="image" :src="'https://www.imumk.ru/svc/coursecover/'+ item.courseId" alt="">
      </div>
      <div class="item__description px-5 py-3">
        <div class="block truncate text-md" :title="item.title">{{ item.subject }}</div>
        <span class="block text-sm">{{ item.grade }} класс</span>
        <span class="block text-xs">{{ item.genre }}</span>
        <a class="text-blue-500" :href="item.shopUrl">Подробнее</a>
        <div v-if="payment" class="text-white text-center bg-blue-500 rounded-lg px-2 py-1 mt-2">{{ item.price }} руб.</div>
        <div v-else class="text-white text-center bg-blue-500 rounded-lg px-2 py-1 mt-2">{{ item.priceBonus }} бонусов</div>
      </div>
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
