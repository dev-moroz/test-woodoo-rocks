<template>
  <div class="container">
    <div class="input-group input-group-sm mb-3 mt-4 mx-auto w-50">
      <div class="input-group-prepend">
        <span class="input-group-text" id="inputGroup-sizing-sm">Search</span>
      </div>
      <input type="text" class="form-control" aria-describedby="inputGroup-sizing-sm" v-model="filter">
    </div>
    <template v-if="isLoaded">
    <div v-if="nothingFound">
      <p class="text-center">Nothing found</p>
    </div>
    <div class="d-flex flex-row flex-wrap align-content justify-content-center mt-2" v-else>
      <div v-for="item,i in sortPosts" :key="i" class="m-2">
        <AppPost :data="item" />
      </div>
    </div>
    </template>
    <div class="text-center mt-3" v-else>
      <div class="spinner-grow" role="status">
        <span class="sr-only"></span>
      </div>
    </div>

  </div>
</template>

<script>
import AppPost from './components/AppPost.vue'

export default {
  name: 'App',
  data(){
    return{
      posts: [],
      filter:''
    }
  },
  components: {
    AppPost
  },
  methods: {
    async getAllData(){
      let posts = await fetch('https://jsonplaceholder.typicode.com/posts')
        .then(response => response.json())
      let users = await fetch('https://jsonplaceholder.typicode.com/users')
        .then(response => response.json())

      this.posts = await posts.map(post => {
        let author = users.filter(user => user.id === post.userId)[0].name
        return {...post, author: author}
        })
    }
  },
  computed:{
    sortPosts(){
      return this.filter.length>0
        ? this.posts.filter(item => item.author.toLowerCase().includes(this.filter.toLowerCase()))
        : this.posts
    },
    nothingFound(){
      return this.sortPosts.length === 0
    },
    isLoaded(){
      return this.posts.length > 0
    }
  },
  mounted(){
    this.getAllData()
  }
}
</script>
