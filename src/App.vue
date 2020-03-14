<template lang="pug">
  div
    Header
    router-view
    Footer
</template>

<script>
// import firebase from 'firebase'
// import store from './store'
import Header from '@/components/base/Header'
import Footer from '@/components/base/Footer'
export default {
  name: 'App',
  components: {
    Header,
    Footer
  },
  data () {
    return {}
  },
  computed: {
    isLoading () {
      return this.$store.getters.loading
    }
  },
  watch: {
    isLoading (newVal, oldVal) {
      if (newVal === true) {
        this.$vs.loading()
      } else {
        this.$vs.loading.close()
      }
    }
  }/* ,
  created () {
    // Обработчик событий "пользователь вошел / вышел"
    firebase.auth().onAuthStateChanged(function (user) {
      if (user) {
        store.dispatch('loggedUser', user)
        store.dispatch('loadMyBooks', user)
      } else {}
    })
  } */
}
</script>

<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #394a1d
  margin-top 60px
</style>
