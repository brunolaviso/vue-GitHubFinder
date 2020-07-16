<template>
  <div id="app">
    <Navbar />

    <div class="container">
      <div class="card card-body">
        <h1>Pesquisar Usuários do GitHub</h1>
        <p class="lead">Digite um nome para encontrar usuários e repositórios</p>
        <input @keyup="getUser" id="search" type="text" class="form-control" required/>
      </div>

      <div class="row mt-3" v-if="user.length !== 0">
        <div class="col-md-4">
          <Profile :user="user" />
        </div>
        <div class="col-md-8">
          <Repo v-for="repo in repos" :key="repo" :repo="repo"/>
        </div>
      </div>

    </div>

  </div>
</template>

<script>

import Navbar from './components/Navbar.vue';
import Profile from './components/Profile.vue';
import Repo from './components/Repo.vue';
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      github:{
        url: 'https://api.github.com/users',
        client_id: 'a5982f8411c812e83a66',
        client_secret: 'a41d7c44b5a646ddbfdc39fe11a746f631c0e012',
        count: 7,
        sort: 'created: asc'
      },
      user: [],
      repos: []
    }
  },
  components: {
    Navbar,
    Profile,
    Repo
  },
  methods: {
    getUser(e){
      const user = e.target.value;
      const { url, client_id, client_secret, count, sort } = this.github;
      axios.get(`${url}/${user}?client_id=${client_id}&client_secret=${client_secret}`).then(({data}) => this.user = data);
      axios.get(`${url}/${user}/repos?per_page=${count}&sort=${sort}&client_id${client_id}&client_secret${client_secret}`).then(({data}) => this.repos = data);

    }
  }
}
</script>