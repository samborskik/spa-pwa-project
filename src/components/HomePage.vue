<template>
  <div>
    <b-button class="float-sm-right" @click='handleChangeFormClick'>
      <span v-if="changeView">Show Favorites</span>
      <span v-else>Back to Search</span>
    </b-button>
    <div v-if="changeView">
      <b-button class="float-sm-right" @click="handleUserLogout" variant="primary">Logout!</b-button>
      <SearchUser :getUserData="this.getUserData" />
    <Loader v-if="isLoading" />
    <User
      v-if="isLoading === false && userData.name !== undefined"
      :userData="this.userData"/>
    </div>
    <div v-else>
      <b-button class="float-sm-right" @click="handleUserLogout" variant="primary">Logout!</b-button>
      <h1>dupa</h1>
    </div>
  </div>
</template>

<script>
import SearchUser from './SearchUser';
import Loader from './Loader';
import User from './User';

export default {
  name: 'HomePage',
  data: () => ({
    changeView: false,
    showFavoritesUsers: false,
    isLoading: false,
    userData: {}
  }),
  components: {
    SearchUser,
    Loader,
    User
  },
  props: {
    setUserSession: Function
  },
  methods: {
    handleChangeFormClick: function() {
      this.changeView = !this.changeView;
    },
    handleUserLogout: function() { 
      localStorage.removeItem("spa-pwa-project")
      this.setUserSession(false);
      alert('Logged out!')
    },
    getUserData: function(user) {
      this.isLoading = true;
      fetch(`https://cors-anywhere.herokuapp.com/https://www.codewars.com/api/v1/users/${user}`)
        .then(res => res.json())
        .then(res => {
          this.userData = res
          this.isLoading = false;
        })
    },
  }
}
</script>