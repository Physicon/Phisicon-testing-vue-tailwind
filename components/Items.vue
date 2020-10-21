<template>
  <div class="wrapper">
    <h1 class="items__title">Items</h1>
    <div class="items__wrapper">
      <item v-for="(item, key) in obtainedData" :key="key" :item="item"></item>
    </div>
    <button @click.prevent="test">test</button>
  </div>
</template>

<script>
import Item from "./Item";

export default {
  name: "Items",
  components: {
    Item
  },
  data() {
    return {
      obtainedData: ''
    }
  },
  mounted() {
    const axios = require('axios').default;
    axios.post('https://krapipl.imumk.ru:8443/api/mobilev1/update', {})
      .then((response) => {
        // console.log(response);
        this.obtainedData = response.data.items
      })

      .catch(function (error) {
        console.log(error);
      });
  },
  methods: {
    test() {
      // console.log(axios)
    }
  }
}
</script>

<style scoped>

</style>
