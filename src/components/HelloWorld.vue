<template>
  <div class="hello">
    <form @submit.prevent="submit">
      <input v-model="song" placeholder="Song">
      <input v-model="artist" placeholder="Artist">
      <input v-model="topic" placeholder="Topic">
      <button type="submit">Submit</button>
    </form>
    <div v-if="apiResponse && !loading">
      <img :src="apiResponse.image" width="512" height="512" />
      <h1>{{ apiResponse.artist }}</h1>
      <h2>{{ apiResponse.song }}</h2>
      <pre>{{ apiResponse.lyrics }}</pre>
    </div>
    <h1 v-if="loading">Loading...</h1>
    <h1 v-else-if="error">Error!</h1>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      song: '',
      artist: '',
      topic: '',
      apiResponse: null,
      loading: false,
      error: false
    }
  },
  methods: {
    submit() {
      const url = `${process.env.VUE_APP_API}/generate/${this.song}/${this.artist}/${this.topic}`
      console.log(url)

      this.loading = true
      this.error = false

      axios.get(url)
          .then(response => {
            this.apiResponse = response.data
            console.log(this.apiResponse)
            this.loading = false
          })
          .catch(error => {
            console.error(error)
            this.error = true
            this.loading = false
          })
    }
  }
}
</script>
