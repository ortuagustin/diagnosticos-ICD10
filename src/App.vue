<template>
  <div class="container shadow-xl">
    <nav>
      <img class="w-full" src="http://www.hospitalespanol.org.ar/images/HE_head_logo_nachoo.jpg">
    </nav>

    <div class="bg-blue-800 px-8 pt-8 flex items-center">
      <input
        v-model="searchString"
        class="rounded-full flex-1 px-8 h-12 mb-8 focus:outline-none focus:shadow-outline text-xl"
        autofocus="true"
        type="search"
        placeholder="Buscar diagnóstico..."
      >

      <div class="h-12 ml-4 text-lg">
        <span class="pr-4 text-white font-bold">Cantidad de resultados</span>
        <input class="w-20 rounded-full text-center" v-model="size" type="text" @input="filter">
      </div>
    </div>

    <div class="bg-white pb-4 pt-2" v-if="count > 0">
      <ul class="mx-10">
        <li class="text-lg leading-relaxed" v-for="hit in hits" :key="hit.id10">
          &bull;
          <span class="mr-2">{{ hit._source.id10 }}</span>
          <span v-html="hit.highlight.dec10[0]"></span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

const http = axios.create({
  baseURL: process.env.VUE_APP_ELASTIC_URL,
})

export default {
  name: 'app',

  data() {
    return {
      searchString: '',
      hits: [],
      response: null,
      size: 20,
    }
  },

  computed: {
    count() {
      return this.hits.length > 0
    },
  },

  methods: {
    filter() {
      if (this.size.replace(/[^0-9]+/g, '') === '') {
        this.size = 20
      }
    },

    async search() {
      this.hits = []
      if (this.searchString.trim().length === 0) return

      const query = {
        size: this.size,

        query : {
          query_string: {
            default_field: 'dec10',
            query: `${this.searchString}~`
          },
        },

        highlight : {
          fields : {
            dec10 : {}
          },
        },
      }

      const params = {
        source: JSON.stringify(query),
        source_content_type: 'application/json',
      }

      this.response = await http.get('/diagnosticos/_search', { params })
      this.data = this.response.data
       if ((this.response.status === 200) && (this.data.hits)) {
        this.hits = this.data.hits.hits
      }
    },
  },

  watch: {
    searchString() {
      this.search()
    },

    size() {
      this.search()
    },
  },
}
</script>

<style src="./assets/app.css"></style>
