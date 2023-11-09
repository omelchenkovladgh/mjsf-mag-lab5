<template>
  <div class="gradient-custom-2">
    <div class="container">
      <nav class="text-center w-100 d-flex justify-content-between align-items-center pt-3 pb-3">
        <div class="">
          <RouterLink class="me-4 r-link me-2" to="/">Todo</RouterLink>
          <RouterLink class="r-link me-2" to="/about">About</RouterLink>
        </div>
        <div class="d-flex justify-content-end align-items-center" v-if="isLoggedIn">
          <p class="user fw-bold mb-0 me-2">{{ displayName }}</p>
          <a class="r-link me-2" @click="signOut">Sign Out</a>
        </div>
      </nav>
    </div>
    <RouterView/>
  </div>
</template>

<script>
import {auth} from './firebase'
import {signOut, onAuthStateChanged} from 'firebase/auth'
import router from "./router";

export default {
  data() {
    return {
      displayName: '',
      isLoggedIn: false
    }
  },
  mounted() {
    onAuthStateChanged(auth, (user) => {
      this.displayName = user ? user.displayName : '';
      this.isLoggedIn = !!user && user.emailVerified;
    })
  },
  methods: {
    signOut() {
      signOut(auth)
          .then(() => {
            router.push('/log-in')
          })
    }
  }
}
</script>
