<template>
  <div>
    LOGGED IN!
    <button @click="userLogOut"> 
      Logout! 
    </button>
  </div>
</template>

<script>
export default {
  name: 'HomePage',
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