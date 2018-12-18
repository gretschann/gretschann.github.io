<template lang="html">
<v-form>
  <v-container fluid>
    <v-layout column wrap>

      <v-flex xs-12>
        <h2 class="display-3 font-weight-black">Search for users</h2>
      </v-flex>

      <!-- <App-search></App-search> -->
      <!-- <v-flex xs12>
       <v-text-field
         label="Type username"
         v-model="username"
         solo>
        </v-text-field>
      </v-flex> -->

      <v-flex xs-6 class="pa-3">
        <v-autocomplete
          v-model="user"
          :items="users"
          :loading="isLoading"
          :search-input.sync="search"
          color="white"
          hide-no-data
          hide-selected
          item-text="login"
          item-value="login"
          label="Public APIs"
          placeholder="Start typing to Search"
          return-object>
        </v-autocomplete>
      </v-flex>

      <v-flex xs12 v-if="user">
       <v-layout row wrap>

         <v-flex xs5 style="padding:8px">
           <div class="">
             <img :src="user.avatar_url" alt="" style="max-width:100%">
             <h4 class="display-1">{{user.login}}</h4>
             <p>Since: {{userProfile.created_at}}</p>
           </div>
         </v-flex>

         <v-flex xs7 style="padding:8px">
           <v-layout row wrap>
             <v-flex>
              <h3 class="font-weight-black pb-4">Saved repositorys</h3>
              <div v-for="repo in userRepos">
                <p class="font-weight-black">
                  <a :href="repo.html_url">
                    {{repo.full_name}}
                  </a>
                </p>
              </div>
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
export default {
  // components: {
  //   AppSeach
  // },
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
    search (val) {
      if(val.length > 4) {
        this.loadData(val);
      }
      // Items have already been requested
      if (this.isLoading) return
      this.isLoading = true
    },
    user () {
      this.getUserRepos()
    }
  },
  methods: {
    loadData (val) {
      // Lazily load input items
      axios.get(`https://api.github.com/search/users?q=${this.search}`)
        .then(res => {
          const { count, entries } = res.data
          this.users = res.data.items
          // this.count = count
          // this.entries = entries
        })
        .catch(err => {
          console.log(err)
        })
        .finally(() => (this.isLoading = false))
    },
    getUserRepos() {
      axios.get(`https:/api.github.com/users/gretschann/repos`)
        .then(repos => {
          this.userRepos = repos.data
        })

      axios.get(`https:/api.github.com/users/gretschann`)
        .then(profile => {
          this.userProfile = profile.data
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
