<template>
  <div>
    <input />
  </div>
</template>

<script>
export default {
  name: 'SearchUser',
  methods: {
    fetchData: (username) => fetch(
      `https://cors-anywhere.herokuapp.com/https://www.codewars.com/api/v1/users/${username}`
    ),
    searchUser: function() { 
      localStorage.removeItem("spa-pwa-project")
      this.setUserSession(false);
      alert('Logged out!')
    }
  }
}
</script>