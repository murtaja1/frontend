<template>
  <b-container v-if="data.length > 0">
    <div v-for="n in data.length" :key="n" class="mb-4">
      <h2 :align="name === 'home' ? 'right' : 'center'" class="mt-2">
        {{ title[n - 1] }}
      </h2>
      <hr class="col-sm-7 col-md-8 col-lg-9 hr-title" />

      <b-card-group class="mt-2">
        <div
          v-for="card in data[n - 1].results"
          :key="card.id"
          class="col-sm-6 col-lg-4 mb-4"
        >
          <b-card align="right" class="border-0 card-scale" no-body>
            <b-link
              :to="'detail/' + homeDetail[n - 1] + '/' + card.id"
              class="text-dark dec"
            >
              <b-card-img
                :src="card.card_image"
                class="rounded mb-3 blur"
                height="170"
                no-body
                img-top
                :alt="card.image"
              />
              <!-- if the name === news -->
              <b-card-text class="dec" v-if="!card.name">
                <h5 class="card-title">{{ card.card_text }}</h5>
              </b-card-text>
              <!-- else -->
              <b-card-text class="dec" v-else>
                <h5 class="card-title">{{ card.name }}</h5>
                {{ card.card_text }}
              </b-card-text>
            </b-link>

            <small class="text-muted">
              <hr class="col-4 hr-time" />
              <b-icon icon="clock" variant="dark"></b-icon>
              {{ getDate(card.last_updated) }}
            </small>
          </b-card>
        </div>
      </b-card-group>
    </div>
    <!-- say you have total-rows=24 and per-page=12means the pagin will be 1_2
     cos for every page will be 12 cards and the total.. is just 24 cards -->
    <b-pagination
      v-if="name !== 'home'"
      v-model="currentPage"
      :total-rows="data[0].count"
      per-page="6"
      prev-text="السابق"
      next-text="التالي"
      limit="4"
      align="center"
      pills
      @input="fetchData"
      hide-goto-end-buttons
      first-number
      last-number
    ></b-pagination>
  </b-container>
</template>

<script>
import shared from '../shared'
export default {
  data() {
    return {
      currentPage: 1,
      // for homeDetail url, it's just like title but english.
      homeDetail: [],
      data: []
    }
  },
  props: {
    name: String,
    title: Array
  },

  watch: {
    // get called whenever the route changes in the same component.
    $route() {
      // take the user to page 1.
      this.currentPage = 1
      this.fetchData()
    }
  },
  methods: {
    fetchData() {
      if (this.name !== 'home') {
        shared
          .fetchData(this.name + `?page=${this.currentPage}&page_size=6`)
          .then((res) => {
            // console.log(res.length)
            this.data = []
            this.homeDetail = [this.name]
            // console.log(this.name)
            this.data.push(res)
          })
      } else {
        this.data = []
        this.homeDetail = ['news', 'universities']
        // not using loop cos sometimes university get called before news
        shared
          .fetchData(this.homeDetail[0] + '?page=1&page_size=6')
          .then((res) => {
            this.data.push(res)

            shared
              .fetchData(this.homeDetail[1] + '?page=1&page_size=6')
              .then((res) => {
                this.data.push(res)
              })
          })
      }
    },

    getDate(last_updated) {
      return shared.getArabDate(last_updated)
    }
  },

  mounted() {
    this.fetchData()
  }
}
</script>

<style lang="scss" scoped>
@media only screen and (min-width: 600px) {
  .card-scale:hover {
    transform: scale(1.02);
  }
  .blur:hover {
    filter: opacity(80%);
  }
}
@media only screen and (max-width: 600px) {
  .hr-title {
    width: 60%;
  }
}
.dec:hover {
  text-decoration: none;
  color: rgb(187, 48, 48);
}
.hr-time {
  position: relative;
  left: 27%;
}
</style>
