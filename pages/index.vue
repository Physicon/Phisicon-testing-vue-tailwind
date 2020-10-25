<template>
  <div class="container mx-auto my-10 wrapper">
    <items></items>
  </div>
</template>

<script>
import Items from "../components/Items";

export default {
  components: {
    Items
  }
}
</script>

<style>
.wrapper {
  max-width: 1000px;
}
</style>
