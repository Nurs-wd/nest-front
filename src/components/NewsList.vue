<template>
  <div v-on:load="load">
    <ul>
      <NewsItem
          v-for="element of news"
          v-bind:el="element"
          :key="element.id"
          v-on:deleteNews="removeNews"
          @edit="edit"
      />
    </ul>
  </div>
</template>

<script>
import NewsItem from '@/components/NewsItem'
export default {
  name:'NewsList',
  props:{
    news:{
      type:Array,
      required:true
    }
  },
  components: {
    NewsItem
  },
     beforeMount() {
     this.load()
   },
  methods: {
    load(){
      this.$emit('load')
    },
    removeNews(id){
      this.$emit('removeNews', id)
    },
    edit(id){
      this.$emit('edit', id)
    }
  }
}
</script>

<style scoped>
ul{
  text-decoration: none;
  list-style: none;
}
</style>
