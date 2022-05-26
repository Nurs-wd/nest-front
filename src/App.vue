<template>
      <h1>News</h1>
  <form @submit.prevent="create">
        <label>Title:</label>
    <input type="text" name="title" v-model="title" required><br>
        <label>Description:</label>
    <input type="text" name="description"  v-model="description"><br>
        <label>Owner Id:</label>
    <input type="text" name="ownerId"  v-model="ownerId" required><br>
    <button type="submit">Submit</button>
  </form>
    <hr>
  <NewsList v-bind:news="news"
  @removeNews="removeNews"
            @edit="edit"
            @load="load"
  />
</template>

<script>
import NewsList from '@/components/NewsList';
export default {
  name: 'App',
    data(){
    return {
      news:[],
      title: '',
      description: '',
      ownerId: ''
    }
  },

  components: {
    NewsList,
  },
  methods: {
    load(){
      fetch('http://localhost:8080/api/rest/news',   {
        method: 'GET',
        headers:{
          "Content-Type": "application/json"
        }
      })
          .then(res =>
            res.json()
          ).then(json => {
            this.news = json
            })
          .catch(err => console.log("Error occurred : ", err))
    },
    removeNews(id) {
      console.log(id)
      this.news = this.news.filter(i => i.id!==id)
      fetch(`http://localhost:8080/api/rest/news/${id}`,   {
        method: 'DELETE',
        headers:{
          "Content-Type": "application/json"
        }
      })
    },
    async create(){
      let element;
      if(this.title.trim() && this.ownerId.trim()) {
        element = {
          title: this.title,
          ownerId :this.ownerId
        }
        if(this.description.trim()){
          element.description = this.description
        }
        const resp = await fetch('http://localhost:8080/api/rest/news',   {
          method: 'POST',
          body: JSON.stringify(element),
          headers: {
            'Content-Type': 'application/json'
          }
         })
        const result = await resp.json()
        console.log(result)
        if(result){
          console.log(result)
          this.news.push(element)
        }
      }
      this.title=''
        this.description=''
        this.ownerId=''
    },
    async edit(id){
      let element ={};
      if(this.title.trim() && this.ownerId.trim()){
          this.news.forEach((i) =>
          {
            if(i.id === id){
              i.title = this.title
              i.description = this.description
              i.ownerId = this.ownerId
              element = {
                title : i.title,
                description : i.description,
                ownerId : i.ownerId
              }
            }
          })
        const req = await fetch(`http://localhost:8080/api/rest/news/${id}`, {
          method: 'PUT',
          body: JSON.stringify(element),
          headers: {
            'Content-Type': 'application/json'
          }
        })
        const resp = await req.json()
        if(resp){
          console.log(resp)
        }
        this.title=''
        this.description=''
        this.ownerId=''
      }
      else{
        window.alert("Please fill the form")
      }

    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
form{
  display: block;

}
form input, form button{
  text-align: center;
  margin: 10px;
}
</style>
