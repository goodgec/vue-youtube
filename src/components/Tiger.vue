<template>
  <div class='tiger'>
    <input type='text' name='name' v-model='searchText' @keypress='search'></input>
    <ul>
      <li v-for='result in results'>
        {{ result.snippet.title }}
      </li>
    </ul>
  </div>
</template>

<script type='text/javascript'>
  import axios from 'axios'

  export default {
    data () {
      return {
        rootUrl: 'https://www.googleapis.com/youtube/v3/search',
        searchText: '',
        key: 'YOUR_KEY_HERE',
        results: []
      }
    },
    computed: {
      params () {
        return {
          part: 'snippet',
          key: this.key,
          q: this.searchText,
          type: 'video'
        }
      }
    },
    methods: {
      search () {
        if (!this.key) {
          throw new Error('need YouTube API key')
        }
        axios.get(this.rootUrl, {
          params: this.params
        })
        .then(response => {
          this.results = response.data.items
        })
      }
    }
  }
</script>

<style type='text/css'>
  .tiger {
    font-weight: bold;
  }
</style>