<template lang="html">
<v-form>
  <v-container fluid>
    <v-layout column wrap>

      <v-flex xs-12>
        <h2 class="display-3 font-weight-black">Search for users</h2>
      </v-flex>

      <!-- START autocomplite -->
      <v-flex xs-6 lg-4  class="pa-3 mb-3" style="background:lightblue">
        <v-autocomplete
          v-model="user"
          :items="users"
          :loading="isLoading"
          :search-input.sync="search"
          color="black"
          hide-no-data
          hide-selected
          item-text="login"
          item-value="login"
          placeholder="Start typing to Search"
          return-object>
        </v-autocomplete>
      </v-flex>

      <!-- START user -->
      <v-flex xs12 v-if="user && userRepos && userProfile">
       <v-layout row wrap>

         <!-- Profile  -->
         <v-flex xs12 sm5 class="pa-2">
           <Profile :profile="userProfile"></Profile>
         </v-flex>

         <!-- Repositories -->
         <v-flex xs12 sm7 class="pa-2">
           <v-layout row wrap>
             <v-flex>
               <Repositories :repositories="userRepos" v-if="userRepos"></Repositories>
             </v-flex>
           </v-layout>
         </v-flex>

       </v-layout>
     </v-flex>

    </v-layout>
  </v-container>
</v-form>
</template>

<script>
import axios from 'axios'
import Repositories from './components/Repositories.vue'
import Profile from './components/Profile.vue'
export default {
  components: {
    Repositories,
    Profile
  },
  data: function () {
    return {
      users: [],
      user: null,
      isLoading: false,
      search: null,
      userRepos: null,
      userProfile: null
    }
  },
  watch: {
    /**
     * Start fetching users only if more that 3 characters are typed
     */
    search (val) {
      if(val.length > 3) {
        this.loadData(val);
      }
      if (this.isLoading) return
      this.isLoading = true
    },
    /**
     * Start fetching user data on user selection
     */
    user () {
      this.getUserData()
    }
  },
  methods: {
    loadData (val) {
      axios.get(`https://api.github.com/search/users?q=${this.search}`)
        .then(res => {
          const { count, entries } = res.data
          this.users = res.data.items
        })
        .catch(err => {
          console.log(err)
        })
        .finally(() => (this.isLoading = false))
    },
    getUserData() {
      axios.get(`https://api.github.com/users/${this.user.login}/repos`)
        .then(repos => {
          this.userRepos = repos.data
        })
        .catch(err => {
          console.log(err)
        })

      axios.get(`https://api.github.com/users/${this.user.login}`)
        .then(profile => {
          this.userProfile = profile.data
        })
        .catch(err => {
          console.log(err)
        })
    }
  }
}
</script>

<style lang="css">
body {
  font-family: 'Roboto'
}
</style>
