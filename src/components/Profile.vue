<template lang="html">
  <div>
    <img :src="profile.avatar_url" alt="" style="max-width:100%">
    <h4 class="display-1">{{profile.name}}</h4>
    <p class="font-weight-black">{{profile.login}}</p>
    <p>On Github since {{registrationDate}}</p>
    <p v-if="profile.location">Located in {{profile.location}}</p>
  </div>
</template>

<script>
export default {
  props: ['profile'],
  computed: {
    registrationDate: function () {
      var date = new Date(this.profile.created_at);
      date = date.toLocaleString('en-EN', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' })
      return date
    }
  }
}
</script>

<style lang="css">
</style>
