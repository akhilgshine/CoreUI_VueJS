<template>
  <div>
    <ul class="list-group">
      <li class="list-group-item" v-for="(item,index) in this.users" v-bind:key="index">
        <a data-toggle="collapse" href="#collapseExample" role="button" aria-expanded="true" aria-controls="collapseExample" @click="getPosts(item.id)">
          {{item.name}}
        </a>
      </li>
    </ul>
    <div class="collapse" id="collapseExample">
      <div class="card card-body">
        <ul class="list-group">
          <button  v-for="(item,index)  in this.posts"  v-bind:key="index" type="button" class="list-group-item list-group-item-action" data-toggle="modal" data-target="#exampleModal" @click="showPost(item)">{{item.title}}</button>
        </ul>
      </div>
    </div>
    <!-- Modal -->
    <div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Post</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close" @click="this.post = {}">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <h5>{{post.title}}</h5>
            <p>{{post.body}}</p>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-dismiss="modal" @click="this.post = {}">Close</button>
          </div>
        </div>
      </div>
    </div>



  </div>


</template>
<script>
    const axios = require('axios');
  export default{
      name:'typography',
      data: function () {
          return {
              users: [],
              posts: [],
              post: {},
              postListOpen: false
          }
      },

      methods: {
          getUsers(){
              var vm = this;
              axios.get('https://jsonplaceholder.typicode.com/users').then(function(success){
                  console.log(success.data)
                  vm.users = success.data
              }).catch(function(error){
                  vm.users = []
              })
          },
          getPosts(id){
              this.posts = [];
              this.postListOpen = !this.postListOpen
              if(!this.postListOpen)
                  return
              console.log(id)
              var vm = this;
              axios.get('https://jsonplaceholder.typicode.com/posts?userId='+id).then(function(success){
                  console.log(success.data)
                  vm.posts = success.data
              }).catch(function(error){
                  vm.posts = []
              })
          },
          showPost(post){
              this.post = post
          }
      },
      beforeMount(){
          this.getUsers()
      }
  }
</script>
