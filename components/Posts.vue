<template>
  <v-card flat>
    <v-layout row wrap>
      <v-flex v-for="(title, index) in titles" :key="index">
        <v-card flat hover class="white pb-2 mb-1 pl-2">
          <v-layout>
            <v-flex xs10>
              <div class="py-2">{{ title.body }}</div>
            </v-flex>
          </v-layout>
        </v-card>
      </v-flex>
    </v-layout>
    <infinite-loading
      v-if="titles.length"
      spinner="spiral"
      @infinite="infiniteScroll"
    ></infinite-loading>
  </v-card>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Posts',
  data() {
    return {
      titles: [],
      page: 1
    }
  },
  computed: {
    url() {
      return 'https://jsonplaceholder.typicode.com/posts?_page=' + this.page
    }
  },
  created() {
    this.fetchData()
  },

  methods: {
    async fetchData() {
      const response = await axios.get(this.url)
      this.titles = response.data
    },
    infiniteScroll($state) {
      setTimeout(() => {
        this.page++
        axios
          .get(this.url)
          .then((response) => {
            if (response.data.length > 1) {
              response.data.forEach((item) => this.titles.push(item))
              $state.loaded()
            } else {
              $state.complete()
            }
          })
          .catch((err) => {
            console.log(err)
          })
      }, 500)
    }
  }
}
</script>

<style scoped>
.theme--light.v-card {
  background-color: #f5f5f5;
}
</style>
