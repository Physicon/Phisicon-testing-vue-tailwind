<template>
  <div class="container mx-auto my-10">
    <div class="wrapper mx-auto">
      <h1 class="items__title mx-1 text-4xl text-center text-gray-700">Витрина</h1>

      <div class="toggle__wrapper mx-1 sticky top-0 right-0 z-10 flex justify-end">
        <label for="toggle__button"
               class="flex items-center cursor-pointer bg-white px-4 py-2 -mr-1 rounded-lg border border-gray-400 shadow-md">
          <div class="mr-3 text-gray-700 font-medium">
            {{ payment ? 'Деньги' : 'Бонусы' }}
          </div>

          <div class="relative">
            <input id="toggle__button" type="checkbox" class="hidden" v-model="payment"/>
            <div class="toggle__line w-10 h-4 bg-gray-400 rounded-full shadow-inner"></div>
            <div class="toggle__dot absolute w-6 h-6 bg-blue-500 rounded-full shadow inset-y-0 left-0"></div>
          </div>
        </label>
      </div>

      <div class="items__filters flex flex-wrap mx-1 mt-2">
        <base-filter :filter="filters.filterSubject" :subject-data="subjectData" @change="change"
                     class="sm:ml-0">
          Все предметы
        </base-filter>

        <base-filter :filter="filters.filterGenre" :subject-data="genreData" @change="change"
                     class="">
          Все жанры
        </base-filter>

        <base-filter :filter="filters.filterGrade" :subject-data="gradeData" @change="change"
                     class="">
          Все классы
        </base-filter>

        <div class="w-full mt-2 sm:w-1/2">
          <label for="searchId"></label>
          <input
            class="appearance-none w-full bg-gray-200 text-gray-700 border border-gray-400 rounded py-2 px-4 leading-tight focus:outline-none focus:bg-white focus:border-gray-500 shadow-md"
            type="text" v-model="search" id="searchId" placeholder="Search...">
        </div>
      </div>

      <div v-if="!loading" class="items__wrapper flex flex-wrap justify-center mx-0 sm:-mx-8 xl:-mx-4">
        <div v-for="(item, key) in filteredList" :key="key"
             class="item__wrapper mt-16 w-full sm:w-1/3 sm:px-8 lg:w-1/4">
          <div class="item border border-gray-500 rounded-lg shadow-md overflow-hidden mx-auto">
            <div class="image__wrapper">
              <img class="image" :src="'https://www.imumk.ru/svc/coursecover/'+ item.courseId" alt="">
            </div>
            <div class="item__description px-5 py-3">
              <div class="block truncate text-md" :title="item.title">{{ item.subject }}</div>
              <span class="block text-sm">{{ item.grade }} класс</span>
              <span class="block text-xs">{{ item.genre }}</span>
              <a class="text-blue-500" :href="item.shopUrl">Подробнее</a>
              <div v-if="payment" class="text-white text-center bg-blue-500 rounded-lg px-2 py-1 mt-2">{{ item.price }}
                руб.
              </div>
              <div v-else class="text-white text-center bg-blue-500 rounded-lg px-2 py-1 mt-2">{{ item.priceBonus }}
                бонусов
              </div>
            </div>
          </div>
        </div>
      </div>
      <base-loader v-else />
    </div>
  </div>
</template>

<script>
import BaseLoader from "../components/BaseLoader";
import BaseFilter from "../components/BaseFilter";

export default {
  components: {
    BaseLoader,
    BaseFilter
  },
  data() {
    return {
      obtainedData: '',
      subjectData: [],
      genreData: [],
      gradeData: [],
      filters: {
        filterSubject: {
          name: 'filterSubject',
          value: 'all',
        },
        filterGenre: {
          name: 'filterGenre',
          value: 'all',
        },
        filterGrade: {
          name: 'filterGrade',
          value: 'all',
        },
      },
      search: '',
      payment: true,
      loading: true
    }
  },
  mounted() {
    const axios = require('axios').default;
    axios.post('https://krapipl.imumk.ru:8443/api/mobilev1/update')
      .then((response) => {
        this.obtainedData = response.data.items
        this.subjectData = [...new Set(this.obtainedData.map(item => item.subject))];
        this.genreData = [...new Set(this.obtainedData.map(item => item.genre))];
        this.gradeData = [...new Set(this.obtainedData.map(item => item.grade))].sort((a, b) => {
          let numA = +a, numB = +b
          if (numA < numB) //sort string ascending
            return -1;
          if (numA > numB)
            return 1;
          return 0;
        });
        this.loading = false
      })
  },
  methods: {
    change(payload, name) {
      this.filters[name].value = payload
    }
  },
  computed: {
    // someFunc(some, filter, item) {
    //   if (filter === 'all') {
    //     return this.obtainedData
    //   }
    //   return this.obtainedData.filter(t => t[item] === filter)
    // },

    filteredBySubject() {
      if (this.filters.filterSubject.value === 'all') {
        return this.obtainedData
      }
      return this.obtainedData.filter(t => t.subject === this.filters.filterSubject.value)
    },
    filteredByGenre() {
      if (this.filters.filterGenre.value === 'all') {
        return this.filteredBySubject
      }
      return this.filteredBySubject.filter(t => t.genre === this.filters.filterGenre.value)
    },
    filteredByGrade() {
      if (this.filters.filterGrade.value === 'all') {
        return this.filteredByGenre
      }
      return this.filteredByGenre.filter(t => t.grade === this.filters.filterGrade.value)
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

<style>
.wrapper {
  max-width: 1000px;
}

.toggle__wrapper {
  top: 20px;
}

.toggle__dot {
  top: -.25rem;
  left: -.25rem;
  transition: all 0.3s ease-in-out;
}

input:checked ~ .toggle__dot {
  transform: translateX(100%);
  background-color: #48bb78;
}

.item {
  width: 170px;
}
</style>
