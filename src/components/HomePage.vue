<template>
  <div>
    LOGGED IN!
    <button @click="userLogOut"> 
      Logout! 
    </button>
    <SearchUser />
  </div>
</template>

<script>
import SearchUser from './SearchUser';


export default {
  name: 'HomePage',
  components: {
    SearchUser
  },
  props: {
    setUserSession: Function
  },
  methods: {
    userLogOut: function() { 
      localStorage.removeItem("spa-pwa-project")
      this.setUserSession(false);
      alert('Logged out!')
    }
  }
}
</script>