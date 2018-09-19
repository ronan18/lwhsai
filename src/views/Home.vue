<template>
<form class="page home" @submit.prevent="submit">
  <label  for="message">Check the sentiment of a sentence by typing in the box below</label>
  <input v-model="message" autocomplete="off" type="text" name="message" placeholder="type here">
  </form>
</template>

<script>
export default {
  name: 'home',
  data() {
    return {
      message: ''
    }
  },
  methods: {
    submit() {
      this.$router.push({name: 'result', params: { query: this.message}})
    }
  }
}
</script>
