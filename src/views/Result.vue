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
  <p class="result__descriptor" :class="colorClass" v-if="results">{{word}}</p>
  <p v-if="results" class="message">"{{this.message}}"</p>
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
const wordbank = {
  high: [
    'super',
    'magnificently',
    'marvelously',
    'outstandingly',
    'superbly',
    'terrificly',
    'wonderfuly',
    'super',
    'groovy',
    'smashingly',
    'positivly',
    'supprisingly'
  ],
  medium: [
    'somewhat',
    'a little',
    'fairly',
    'kind of',
    'moderately',
    'partially',
    'pretty',
    'rather',
    'slightly',
    'incompleatly',
    'more or less',
    'ratherish',
    'to a degree',
    'nearly',
    'almost'
  ],
  positive: [
    'positive',
    'groovy',
    'good',
    'positive',
    'kind',
    'amiable',
    'cordial',
    'courteous',
    'friendly',
    'gracious',
    'kindly',
    'loving',
    'neighborly',
    'big'
  ],
  negative: [
    'callous',
    'mean',
    'neagtive',
    'dirty',
    'evil',
    'malicious',
    'nasty',
    'rough',
    'ugly',
    'vicious',
    'vile',
    'sinking',
    'sour',
    'churlish',
    'snide',
    'disagreeable'
  ]
}
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
      let randNum = Math.floor(Math.random() * 2)
      console.log(randNum)
      if (randNum == 0) {
        fetch('http://api.icndb.com/jokes/random?escape=javascript')
          .then(response => response.json())
          .then(data => {
            message = data.value.joke
            setTimeout(() => {
              this.results = this.getData(message)
              this.message = message
            }, 500)
          })
      } else if (randNum == 1) {
        fetch('http://ron-swanson-quotes.herokuapp.com/v2/quotes')
          .then(response => response.json())
          .then(data => {
            message = data[0]
            //console.log(data)
            setTimeout(() => {
              this.results = this.getData(message)
              this.message = message
            }, 500)
          })
      }
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
       fetch(`http://127.0.0.1:5000/v98745/theonlyfuckingurl?sentence=${message}`)
          .then(response => response.json())
          .then(data => {
            return data
          })
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
    },
    word() {
      let num = this.results.sentiment
      let first, last
      if (num >= 50) {
        last =
          wordbank.positive[
            Math.floor(Math.random() * wordbank.positive.length)
          ]

        if (num >= 75) {
          first =
            wordbank.high[Math.floor(Math.random() * wordbank.high.length)]
        } else {
          first =
            wordbank.medium[Math.floor(Math.random() * wordbank.medium.length)]
        }
        return first + ' ' + last
      } else {
        last =
          wordbank.negative[
            Math.floor(Math.random() * wordbank.positive.length)
          ]

        if (num <= 25) {
          first =
            wordbank.high[Math.floor(Math.random() * wordbank.high.length)]
        } else {
          first =
            wordbank.medium[Math.floor(Math.random() * wordbank.medium.length)]
        }
        return first + ' ' + last
      }
    }
  }
}
</script>
