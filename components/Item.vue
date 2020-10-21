<template>
  <div class="item__wrapper">
    <div class="item">
      {{ item }}
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
    }
  }
}
</script>

<style scoped>

</style>
