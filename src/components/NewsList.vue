<template>
    <ul class="news__list">
      <li v-for="article in articles"  class="news__items"> {{ article.title }}
      <img class="imgs" :src= "articles.urltoimages"/>
      {{article.description}} </li>
      
    </ul>

    <form @submit.prevent="searchNews" class="d-flex flex-column justify-content-center">
     <div class="input-group mx-sm-3 mb-2">
       <label class="visually-hidden" for="search">Search</label>
      <input type="search" name="search" v-model="searchTerm" id="search" class="form-control mb-2 mr-sm-2" placeholder="Enter search term here" />
      <button class="btn btn-primary mb-2">Search</button>
     </div>
    <p>You are searching for {{ searchTerm }}</p>
    </form>
    

</template>

 <script>
    export default {
      data() {
        return {
           articles: [],
           searchTerm: ''
        };
      },

      created() {
          let self = this;86

          fetch('https://newsapi.org/v2/top-headlines?country=us',
    {
      headers: {
          Authorization: `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}`
      }
    })
              .then(function(response) {
                return response.json();
              })
              .then(function(data) {
                console.log(data);
                self.articles = data.articles;
              });
      }, 
      methods: {
        searchNews() {
          let self = this;

          fetch('https://newsapi.org/v2/everything?q='+
    self.searchTerm + '&language=en', { 
      headers: {
          'Authorization': `Bearer $
    {import.meta.env.VITE_NEWSAPI_TOKEN}`,
      }
    })
           .then(function(response) {
              return response.json();
            })
            .then(function(data) {
               console.log(data);
               self.articles = data.articles;  
            });
        }
      }      
    };
 </script>

 <style scoped>
   .news__item{
    display: grid;
    grid-template-columns:repeat(3, 150px);
    grid-gap:0.2rem;
    width: 300px;
    height: 300px;
    
    
  }
   .imgs{
    display: grid;
    grid-template-columns:repeat(3, 150px);
    object-fit:cover;
 	  border-radius:50px;
    width:600px;
 	  height: 22vw;
 	  
 }
 </style>