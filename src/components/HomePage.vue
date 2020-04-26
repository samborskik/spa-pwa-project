<template>
  <div>
    <b-button @click="handleUserLogout"> 
      Logout! 
    </b-button>
    <SearchUser :getUserData="this.getUserData" />
    <Loader v-if="isLoading" />
    <User
      v-if="isLoading === false && userData.name !== undefined"
      :userData="this.userData"
    />
  </div>
</template>

<script>
import SearchUser from './SearchUser';
import Loader from './Loader';
import User from './User';

export default {
  name: 'HomePage',
  data: () => ({
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