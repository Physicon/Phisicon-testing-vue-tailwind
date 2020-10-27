<template>
  <div
    class="relative w-48 mt-1 mx-auto sm:mt-2 md:mr-2 md:ml-0">
    <select
      class="focus:outline-none block w-full appearance-none bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline"
      v-model="filterValue" @change="$emit('change', filterValue, filter.name)">
      <option value="all">
        <slot></slot>
      </option>
      <option v-for="(subject, key) in subjectData" :value="subject">
        {{ (filter.name === 'filterGrade') ? editedOption(subject) : subject }}
        {{ (filter.name === 'filterGrade') ? ' класс' : '' }}
      </option>
    </select>
    <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700">
      <svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
        <path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"/>
      </svg>
    </div>
  </div>
</template>

<script>
export default {
  name: "BaseFilter",
  props: {
    filter: {
      type: Object,
      required: true
    },
    subjectData: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      filterValue: 'all'
    }
  },
  methods: {
    editedOption(data) {
      return data.split(';').join(', ')
    },
  },
}
</script>

<style scoped>

</style>
