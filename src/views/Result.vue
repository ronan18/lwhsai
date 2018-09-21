<template>
<div class="page result">
  <div v-if="!results" class="loading">
    <div class="loading-bar"></div>
    <div class="loading-bar"></div>
    <div class="loading-bar"></div>
    <div class="loading-bar"></div>
  </div>
  <p v-if="!results" class="loading__text">Calculating</p>
  <p v-if="results" class="result__num" :class="colorClass">{{round(results.sentiment)}}</p>
  <p v-if="results" class="message">{{this.message}}</p>
  <div @keyup.space="reload()" v-if="results" class="sliderContainer">
    <div class="slider__label__row">
      <label class="slider__label">negative</label>
      <label class="slider__label">positive</label>
    </div>

    <input v-model="results.sentiment"  type="range" class="slider" disabled>
  </div>
    <p v-if="results" class="results__time">computed in {{results.time}}s</p>
    <router-link to="/" v-if="results" class="btn">Run Again</router-link>
  </div>
</template>

<script>
export default {
  name: 'home',
  data() {
    return {
      message: '',
      results: null
    }
  },
  mounted() {
    let message = this.$route.params.query
    if (!message) {
      this.$router.push('/')
    }
    if (message == 'chuckNorris') {
      fetch('http://api.icndb.com/jokes/random?escape=javascript')
        .then(response => response.json())
        .then(data => {
          message = data.value.joke
          setTimeout(() => {
            this.results = this.getData(message)
            this.message = message
          }, 500)
        })
    } else {
      setTimeout(() => {
        this.results = this.getData(message)
        this.message = message
      }, 500)
    }

    window.addEventListener('keypress', e => {
      if (
        (e.code == 'Space' && this.$route.path == '/result') ||
        (e.code == 'Enter' && this.$route.path == '/result')
      ) {
        this.reload()
      }
    })
  },
  methods: {
    getData(message) {
      return {
        sentiment: Math.random() * 100,
        time: Math.random().toFixed(2)
      }
    },
    round(i) {
      return Math.round(i)
    },
    reload() {
      this.$router.push('/')
    }
  },
  computed: {
    colorClass() {
      return {
        green: this.results.sentiment >= 66,
        yellow: this.results.sentiment < 66 && this.results.sentiment > 33,
        red: this.results.sentiment <= 33
      }
    }
  }
}
</script>
