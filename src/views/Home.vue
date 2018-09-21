<template>
<form class="page home" @submit.prevent="submit">
  <label  for="message">Check the sentiment of a sentence by typing in the box below</label>
  <input v-model="message" autocomplete="off" type="text" name="message" placeholder="type here" autofocus="true" v-focus>
  <p class="error">{{error}}</p>
  </form>
</template>

<script>
export default {
  name: 'home',
  data() {
    return {
      message: '',
      error: ''
    }
  },
  methods: {
    submit() {
      if (this.message.length > 5) {
        this.$router.push({
          name: 'result',
          params: {
            query: this.message
          }
        })
      } else {
        this.error = "please make sure your sentence has more than 5 characters"
      }

    }
  }
}
</script>
