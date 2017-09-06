<template>
  <div id="app" class="container">
    <div class="row">
      <div class="page-header">
        <h1> Vue Js + FireBase :Ikhsan</h1>
      </div>
    </div>

    <div class="row">
      <div class="col-sm-6">
        <div class="well bs-component">
          <form id="form" class="form-horizontal"  v-on:submit.prevent="addBook">
            <fieldset>
              <legend>Add Book</legend>
              <div class="form-group">
                <label for="bookTitle" class="col-sm-2 control-label">Title:</label>
                <div class="col-lg-10">
                  <input type="text" id="bookTitle" class="form-control" v-model="newBook.title">
                </div>
              </div>
              <div class="form-group">
                <label for="bookTitle" class="col-sm-2 control-label">Author:</label>
                <div class="col-lg-10">
                  <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author">
                </div>
              </div>
              <div class="form-group">
                <label for="bookUrl" class="col-sm-2 control-label">URL:</label>
                <div class="col-lg-10">
                  <input type="text" id="bookUrl" class="form-control" v-model="newBook.url">
                </div>
              </div>
              <div class="form-group">
                <div class="col-sm-10 col-sm-offset-2"> 
                  <input type="submit" class="btn btn-primary" value="Add Book">
                </div>
              </div>          
            </fieldset>
          </form>
        </div>
      </div>  
      
      <div class="col-sm-6">
        <div class="panel panel-default">
          <div class="panel-heading">
            <h3>Books Lists</h3>
          </div>
          <div class="panel-body">
            <table class="table table-striped table-hover">
              <thead>
                <tr>
                  <th>Title</th>
                  <th>Author</th>
                  <th>Delete</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="book in books">
                  <td> <a v-bind:href="book.url">{{ book.title }}</a>  </td>
                  <td> {{ book.author }} </td>
                  <td> <span class="glyphicon glyphicon-trash" v-on:click="removeBook(book)"></span> </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
    <!-- <router-view></router-view> -->
  </div>
</template>

<script>

import Firebase from 'firebase'

import toastr from 'toastr'

let config = {
  apiKey: 'AIzaSyCK-a6Y_ZY0H1iGcgBzemwk7WzENNluO8k',
  authDomain: 'vuejsfirebase-c59c6.firebaseapp.com',
  databaseURL: 'https://vuejsfirebase-c59c6.firebaseio.com',
  projectId: 'vuejsfirebase-c59c6',
  storageBucket: 'vuejsfirebase-c59c6.appspot.com',
  messagingSenderId: '250845523163'
}

let app = Firebase.initializeApp(config)
let db = app.database()

let booksRef = db.ref('books')

export default {
  name: 'app',
  firebase: {
    books: booksRef
  },

  data () {
    return {
      newBook: {
        title: '',
        author: '',
        url: 'http://'
      }
    }
  },
  methods: {
    addBook: function () {
      booksRef.push(this.newBook)
      toastr.success('Book added')
      this.newBook.title = ''
      this.newBook.author = ''
      this.newBook.url = 'http://'
    },
    removeBook: function (book) {
      booksRef.child(book['.key']).remove()
      toastr.success('Book removed')
    }
  }
}

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
