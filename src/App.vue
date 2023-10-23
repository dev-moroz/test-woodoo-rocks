<template>
  <div class="container">
    <div class="input-search my-3 m-auto d-flex">
      <label for="search">Search: </label>
      <input id="search" type="text" v-model="filter" class="w-100">
    </div>
    <template v-if="isLoaded">
      <div v-if="nothingFound">
        <p class="text-center">Nothing found</p>
      </div>
      <div class="d-flex flex-row flex-wrap mt-2 justify-content-between;"  v-else>
        <div v-for="item in sortPosts" :key="item.id" class="mx-2 my-1" style="flex-basis: 31%;">
          <AppPost :data="item" />
        </div>
      </div>
    </template>
    <div class="text-center my-5" v-if="isLoadData">
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
      filter:'',
      showedPosts: [],
      slice: 10,
      isLoadData: false
    }
  },
  components: {
    AppPost
  },
  watch: {
    isLoadData: {
      handler(value){
        if(value) setTimeout(() => this.setPartData(), 2000)
      }
    }
  },
  methods: {
    async getAllData(){
      this.isLoadData = true
      const posts = await fetch('https://jsonplaceholder.typicode.com/posts')
        .then(response => response.json())
      const users = await fetch('https://jsonplaceholder.typicode.com/users')
        .then(response => response.json())

      this.posts = posts.map(post => {
        const author = users.find(user => user.id === post.userId).name
        return {...post, author: author}
        })

      setTimeout(() => this.setPartData(), 1000)
    },

    setPartData(){
      this.showedPosts = this.posts.slice(0, this.slice)
      this.slice += 10
      this.isLoadData = false
    },

    loadData(){        
      const scrolled = window.scrollY
      const height = document.body.offsetHeight
      const screenHeight = window.innerHeight
      const partOfHeight = (height - screenHeight) - 100

      if(scrolled > partOfHeight) this.isLoadData = true
    }
  },
  computed:{
    sortPosts(){
      const filter = this.filter.toLowerCase()
      return this.filter.length
        ? this.showedPosts.filter(post => post.title.toLowerCase().includes(filter) 
            || post.body.toLowerCase().includes(filter)
            || post.author.toLowerCase().includes(filter)
          )
        : this.showedPosts
    },
    nothingFound(){
      return !this.sortPosts.length
    },
    isLoaded(){
      return this.showedPosts.length
    }
  },
  mounted(){
    this.getAllData()
    window.addEventListener('scroll', () => this.loadData())
  }
}
</script>

<style>
.input-search{
  width: 90% !important;
  border-radius: 8px;
  padding: 6px;
  gap: 10px;
}
</style>