<template>
  <div class="wrapper">
    <h1 class="items__title mx-1 text-4xl text-center text-gray-700">Витрина</h1>

    <div class="toggle__wrapper mx-1 sticky top-0 right-0 z-10">
      <button @click.prevent="payment = !payment"
              class="ml-auto bg-blue-500 hover:bg-blue-700 text-white block py-2 px-4 rounded shadow-md">
        {{ payment ? 'Бонусы' : 'Деньги' }}
      </button>
    </div>

    <div class="items__filters flex flex-wrap mx-1 mt-2">
      <div
        class="subject__wrapper relative mt-2 lg:mt-0 lg:mr-2 block w-full sm:w-1/2 sm:-ml-2 sm:pl-2 lg:w-auto lg:ml-0 lg:pl-0 lg:inline-block">
        <label for="subjectId"></label>
        <select
          class="items__subject block w-full appearance-none bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline"
          id="subjectId" v-model="filterSubject">
          <option value="all">Все предметы</option>
          <option v-for="(subject, key) in subjectData" :value="subject">{{ subject }}</option>
        </select>
        <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700">
          <svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
            <path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"/>
          </svg>
        </div>
      </div>

      <div
        class="genre__wrapper relative mt-2 lg:mt-0 lg:mr-2 block w-full sm:w-1/2 sm:ml-2 sm:pl-2 lg:w-auto lg:ml-0 lg:pl-0 lg:inline-block">
        <label for="genreId"></label>
        <select
          class="items__genre block w-full appearance-none bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline"
          id="genreId" v-model="filterGenre">
          <option value="all">Все жанры</option>
          <option v-for="(genre, key) in genreData" :value="genre">{{ genre }}</option>
        </select>
        <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700">
          <svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
            <path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"/>
          </svg>
        </div>
      </div>

      <div
        class="grade__wrapper relative mt-2 lg:mt-0 lg:mr-2 block w-full sm:w-1/2 sm:-ml-2 sm:pl-2 lg:w-auto lg:ml-0 lg:pl-0 lg:inline-block">
        <label for="gradeId"></label>
        <select
          class="items__grade block w-full appearance-none bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline"
          id="gradeId" v-model="filterGrade">
          <option value="all">Все классы</option>
          <option v-for="(grade, key) in gradeData" :value="grade">{{ grade }}</option>
        </select>
        <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700">
          <svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
            <path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"/>
          </svg>
        </div>
      </div>

      <div
        class="search__wrapper relative mt-2 lg:mt-0 block w-full sm:w-1/2 sm:ml-2 sm:pl-2 lg:w-auto lg:ml-0 lg:pl-0 lg:inline-block lg:ml-auto">
        <label for="searchId"></label>
        <input
          class="appearance-none w-full bg-gray-200 text-gray-700 border border-gray-200 rounded py-2 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500 shadow-md"
          type="text" v-model="search" id="searchId" placeholder="Search...">
      </div>
    </div>

    <div v-if="!loading" class="items__wrapper flex flex-wrap justify-center -mx-1 mt-8">
      <item v-for="(item, key) in filteredList" :key="key" :item="item" :payment="payment"></item>
    </div>
    <loader v-else></loader>
  </div>
</template>

<script>
import Item from "./Item";
import Loader from "./Loader";

export default {
  name: "Items",
  components: {
    Item,
    Loader
  },
  data() {
    return {
      obtainedData: '',
      subjectData: [],
      genreData: [],
      gradeData: [],
      filterSubject: 'all',
      filterGenre: 'all',
      filterGrade: 'all',
      search: '',
      payment: true,
      loading: true
    }
  },
  mounted() {
    const axios = require('axios').default;
    axios.post('https://krapipl.imumk.ru:8443/api/mobilev1/update', {})
      .then((response) => {
        setTimeout(() => {
          this.obtainedData = response.data.items
          this.subjectData = [...new Set(this.obtainedData.map(item => item.subject))];
          this.genreData = [...new Set(this.obtainedData.map(item => item.genre))];
          this.gradeData = [...new Set(this.obtainedData.map(item => item.grade))];
          this.loading = false
        }, 1000)
      })

      .catch(function (error) {
        console.log(error);
      });
  },
  methods: {
    test() {
      // console.log(axios)
    }
  },
  computed: {
    filteredBySubject() {
      if (this.filterSubject === 'all') {
        return this.obtainedData
      }
      return this.obtainedData.filter(t => t.subject === this.filterSubject)
    },
    filteredByGenre() {
      if (this.filterGenre === 'all') {
        return this.filteredBySubject
      }
      return this.filteredBySubject.filter(t => t.genre === this.filterGenre)
    },
    filteredByGrade() {
      if (this.filterGrade === 'all') {
        return this.filteredByGenre
      }
      return this.filteredByGenre.filter(t => t.grade === this.filterGrade)
    },
    filteredList() {
      if (this.search) {
        return this.filteredByGrade.filter(post => post.title.toLowerCase().includes(this.search.toLowerCase()))
      }
      return this.filteredByGrade
    }
  }
}
</script>

<style scoped>
.toggle__wrapper {
  top: 20px;
}
</style>
