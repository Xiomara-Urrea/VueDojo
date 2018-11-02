<template>
  <div id="app" class="container">
    <h1>Vue & Firebase App</h1>

    <div class="card">

      <div class="card-header">
        <h3>Add a link</h3>
      </div>

      <div class="card-body">
        <form v-on:submit.prevent = "addLink" class = form-inline>
        <div class="form-group">
          <label for="">Title:  </label>

          <input
            v-model = "newLink.title"
            class = "form-control"
            placeholder="Title"
            type="text">
        </div>


        <div class="form-group">
          <label for="">Author:  </label>
          <input
            v-model = "newLink.author"
            class = "form-control"
            placeholder="Author"
            type="text">
        </div>

        <div class="form-group">
          <label for="">Url:  </label>

          <input
            v-model = "newLink.url"
            class = "form-control"
            placeholder="Url"
            type="text">
        </div>

        <input type="submit" class = "btn btn-success" value="Add A Link">
        </form>
      </div>
    </div>

    <hr>

    <div class="card">
      <div class="card-header">
        <h3 class = "card-title">Links list</h3>
      </div>
      <div class="card-body">
        <table class = "table table-stripped">
          <thead>      
            <tr>
              <th>Title</th>
              <th>Author</th>
              <th>Delete</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for = "link in links">
              <td>
                <a target="_blank" v-bind:href="link.url">{{ link.title }}</a>
              </td>
              <td>
                {{ link.author }}
              </td>
              <td>
                <button
                  v-on:click  = "deleteLink(link)"
                  class="btn btn-danger">
                  <i class="fas fa-trash-alt"></i>
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>

import Firebase from 'firebase';
import toastr from 'toastr';

let config = {
  apiKey: "AIzaSyAKCY8pNkIdnPsMjP02xZMjYl-GlCaCmKw",
    authDomain: "dojo-vue-c9503.firebaseapp.com",
    databaseURL: "https://dojo-vue-c9503.firebaseio.com",
    projectId: "dojo-vue-c9503",
    storageBucket: "",
    messagingSenderId: "77979330530"
}

let app = Firebase.initializeApp(config);
let db = app.database();

let linksRef = db.ref('links');

export default {
  name: 'App',
  firebase: {
    links: linksRef
  },
  data(){
    return {
      newLink: {
        title: '',
        author: '',
        url: ''
      }
    }
  },
  methods: {
    addLink: function(){
      linksRef.push(this.newLink);
      this.newLink.title = '';
      this.newLink.author = '';
      this.newLink.url = '';
    },
    deleteLink: function(link){
      linksRef.child(link['.key']).remove();
      toastr.success('Link removed');
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
