<template>
  <div class="animated fadeIn">
    <b-row>
      <b-col md="6">
        <b-card header-tag="header" footer-tag="footer">
          <div slot="header">
            <i class="fa fa-align-justify"></i>
            <strong>Users</strong>
            <div class="card-header-actions">
              <a
                href="https://bootstrap-vue.js.org/docs/components/list-group"
                class="card-header-action"
                rel="noreferrer noopener"
                target="_blank"
              >
                <small class="text-muted">docs</small>
              </a>
            </div>
          </div>
          <b-list-group>
            <b-list-group-item v-for="(item,index) in this.users" v-bind:key="index">
              <a
                v-b-toggle="'#blogs-'+item.id"
                data-toggle="collapse"
                href="javascript:void(0);"
                role="button"
                aria-expanded="true"
                aria-controls="collapseExample"
                @click="getPosts(item.id)"
              >{{item.name}}</a>
              <b-collapse :id="'#blogs-'+item.id">
                <b-card
                  v-for="(post, index) in posts"
                  v-bind:key="index"
                  @click="showPost(post)"
                  class="mb-2"
                >{{post.title}}</b-card>
              </b-collapse>
            </b-list-group-item>
          </b-list-group>
        </b-card>
      </b-col>
    </b-row>
    <b-modal :title="post.title" v-model="myModal" @ok="myModal = false">{{post.body}}</b-modal>
  </div>
</template>

<script>
const axios = require("axios");
export default {
  name: "dashboard",
  data: function() {
    return {
      users: [],
      posts: [],
      post: {},
      postListOpen: false,
      myModal: false
    };
  },

  methods: {
    getUsers() {
      var vm = this;
      axios
        .get("https://jsonplaceholder.typicode.com/users")
        .then(function(success) {
          console.log(success.data);
          vm.users = success.data;
        })
        .catch(function(error) {
          vm.users = [];
        });
    },
    getPosts(id) {
      this.posts = [];
      this.postListOpen = !this.postListOpen;
      if (!this.postListOpen) return;
      console.log(id);
      var vm = this;
      axios
        .get("https://jsonplaceholder.typicode.com/posts?userId=" + id)
        .then(function(success) {
          console.log(success.data);
          vm.posts = success.data;
        })
        .catch(function(error) {
          vm.posts = [];
        });
    },
    showPost(post) {
      this.myModal = true;
      this.post = post;
    }
  },
  beforeMount() {
    this.getUsers();
  }
};
</script>

<style>
/* IE fix */
#card-chart-01,
#card-chart-02 {
  width: 100% !important;
}
</style>
