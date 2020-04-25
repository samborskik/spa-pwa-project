<template>
  <div>
    <p>{{ userData.username }}</p>
    <p>{{ userData.name }}</p>
    <p>{{ userData.honor }}</p>
    <p>{{ userData.clan }}</p>
    <p>{{ userData.leaderboardPosition }}</p>
  </div>
</template>

<script>

export default {
  name: 'User',
  props: {
    userData: Object
  },
}
</script>