<template>
 
    <nav class="navbar navbar-expand navbar-dark bg-dark">
      <router-link :to="{name:'home'}" lass="navbar-brand">Hannah</router-link>
      <div class="navbar-nav mr-auto">
        <li class="nav-item">
          <router-link :to="{name:'home'}" class="nav-link">
            <font-awesome-icon icon="home" /> Home
          </router-link>
        </li>
        <li v-if="showManagerBoard" class="nav-item">
          <router-link :to="{name:'manager'}" class="nav-link">Manger Board</router-link>
        </li>
        <li v-if="showClerkBoard" class="nav-item">
          <router-link :to="{name:'clerk'}" class="nav-link">Clerk Board</router-link>
        </li>
        <li v-if="currentUser" class="nav-item">
          <router-link  :to="{name:'user'}" class="nav-link">User</router-link>
        </li>
      </div>
      <div v-if="!currentUser" class="navbar-nav ml-auto">
        <li class="nav-item">
          <router-link :to="{name :'register'}" class="nav-link">
            <font-awesome-icon icon="user-plus" /> Sign Up
          </router-link>
        </li>
        <li class="nav-item">
          <router-link :to="{name:'login'}" class="nav-link">
            <font-awesome-icon icon="sign-in-alt" /> Login
          </router-link>
        </li>
      </div>
      <div v-if="currentUser" class="navbar-nav ml-auto">
        <li class="nav-item">
          <router-link :to="{name:'profile'}" class="nav-link">
            <font-awesome-icon icon="user" />
            {{ currentUser.username }}
          </router-link>
        </li>
        <li class="nav-item">
          <a class="nav-link" @click.prevent="logOut">
            <font-awesome-icon icon="sign-out-alt" /> LogOut
          </a>
        </li>
      </div>
    </nav>
    
</template>
<script>
export default {
  computed: {
    currentUser() {
      return this.$store.state.auth.user;
    },
    showManagerBoard() {
      if (this.currentUser && this.currentUser['roles']) {
        return this.currentUser['roles'].includes('ROLE_MANAGER');
      }
      return false;
    },
    showClerkBoard() {
      if (this.currentUser && this.currentUser['roles']) {
        return this.currentUser['roles'].includes('ROLE_CLERK');
      }
      return false;
    }
  },
  methods: {
    logOut() {
      this.$store.dispatch('auth/logout');
      this.$router.push({name:'login'});
    }
  }
};
</script>
