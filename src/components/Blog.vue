<template>
  <div class="container">
      <table>
  <tr>
    <th>Title</th>
    <th>Category</th>
    <th>Author</th>
  </tr>

    <single-post v-bind:key="post.title" v-for="post in allBlogPosts" :title="post.title" :category="post.category" :author="post.author"></single-post>
    
</table>
    <!-- <form action="/localhost:3001" method="POST"> -->
        <input name="title" v-model="newTitle" type="text">
        <input name="category" v-model="newCategory" type="text">
        <input name="author" v-model="newAuthor" type="text">
        <p>Title:  {{newTitle}}</p>
        <p>Category:  {{newCategory}}</p>
        <p>Author:  {{newAuthor}}</p>
        <button @click="addPost">Send</button>
    <!-- </form> -->
  </div>
</template>

<script>
import io from 'socket.io-client';
import singlePost from './singlePost.vue'


export default {
    name: 'app',
    components: {
    singlePost
  },
    data() {
        return {
            allBlogPosts:[],
            newTitle: "",
            newCategory: "",
            newAuthor: "",

            socket : io('localhost:3001')
        }
    },
    methods: {
        addPost(e) {
            this.socket.emit('addPost', {
                title: this.newTitle, category: this.newCategory, author: this.newAuthor
            });
            this.newTitle= "";
            this.newCategory= "";
            this.newAuthor= "";
            console.log("addpost se desio")
        },
    },
    mounted() {
       this.socket.on('retrievePost', (data) => {
           console.log("retrived se desio")
            console.log(data);
            this.allBlogPosts.push(data);
        });
    },
}
</script>

<style>
table {
    font-family: arial, sans-serif;
    border-collapse: collapse;
    width: 100%;
}

td, th {
    border: 1px solid #dddddd;
    text-align: left;
    padding: 8px;
}

tr:nth-child(even) {
    background-color: #dddddd;
}
</style>