<template>
  <div v-theme:column="'narrow'" id="show-blog">
      <h1>博客总览</h1>
      <input type="text" placeholder="搜索" v-model="search">
      <div class="single-blog" v-for="blog in filteredBlogs" :key="blog.id">
          <router-link v-bind:to="'/blog/' + blog.id">
              <h2 v-rainbow>{{blog.title | to-uppercase}}</h2>
          </router-link>
          <article>
              {{blog.content | snippet}}
          </article>
      </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'show-blog',
  data(){
      return{
          blogs:[],
          search:""
      }
  },
  created(){
      axios.get("/posts1.json")
        .then((data) => {
            return data.data;
            // return data.json();
            // console.log(data.json())
            // this.blogs = data.body.slice(0,10);
        })
        .then((data) => {
            var blogsArray = [];
            for(let key in data){
                // console.log(key);
                // console.log(data[key]);
                data[key].id = key;
                blogsArray.push(data[key]);
            }
            // console.log(blogsArray);
            this.blogs = blogsArray;
            // console.log(this.blogs);
        })
  },
  computed:{
      filteredBlogs(){
          return this.blogs.filter((blog) => {
              return blog.title.match(this.search);
          })
      }
  },
  filters:{
      toUppercase(value){
          return value.toUpperCase();
      },
      snippet(value){
          return value.slice(0,100) + '...';
      }
  },
  directives:{
      'rainbow':{
          bind(el,binding,vnode){
              el.style.color = "#" + Math.random().toString(16).slice(2,8);
          }
      }
  }
}
</script>

<style>
#show-blog{
    max-width: 800px;
    margin: 0 auto;
}
.single-blog{
    padding: 20px;
    margin: 20px 0;
    box-sizing: border-box;
    background: #eee;
    border: 1px dotted #aaa;
}
#show-blog a{
    color: #444;
    text-decoration: none;
}
input[type=text]{
    padding: 8px;
    width: 100%;
    box-sizing: border-box;
}
</style>
