<template lang="pug">
  #mainDiv
    .text Домашняя библиотека
      div
        span.olive &nbsp; - сообщество истинных ценителей литературы. Здесь вы можете найти труд по душе или поделиться книгой с другими читателями.
    #firebaseui-auth-container(v-if="!checkUser")
</template>

<script>
import firebase from 'firebase'
import * as firebaseui from 'firebaseui'
import store from '../store'
import '../../node_modules/firebaseui/dist/firebaseui.css'
export default {
  name: 'Home',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  computed: {
    checkUser () {
      return store.getters.checkUser
    }
  },
  mounted () {
    if (!this.checkUser) {
      // Инициализация и старт представления аутентификации Гугл
      var uiConfig = {
        signInSuccessUrl: '/',
        signInOptions: [
          firebase.auth.GoogleAuthProvider.PROVIDER_ID
        ]
      }
      let ui = firebaseui.auth.AuthUI.getInstance()
      if (!ui) {
        ui = new firebaseui.auth.AuthUI(firebase.auth())
      }
      ui.start('#firebaseui-auth-container', uiConfig)
    }
  }
}
</script>

<style scoped lang="stylus">
  #mainDiv
    background url('../assets/book-bg-2.jpg') center center fixed
    -webkit-background-size: cover
    -moz-background-size: cover
    -o-background-size: cover
    background-size: cover
    position: fixed
    width 100%
    height 100%
  .text
    text-align justify
    font-family Helvetica, sans-serif
    font-weight lighter
    font-size 2em
    display block
    overflow hidden
    margin-top 5%
    width 50%
    margin-left 25%
    color #394a1d
    text-align justify
  .olive
    color #aba87b
  .text last-of-type
    width 0px
    animation reveal 7s infinite
  .text last-of-type span
    margin-left -355px
    animation slidein 7s infinite
  @keyframes showup
    0%
      opacity 0
    20%
      opacity 1
    80%
      opacity 1
    100%
      opacity 0
  @keyframes slidein
    0%
      margin-left -800px
    20%
      margin-left -800px
    35%
      margin-left 0px
    100%
      margin-left 0px
  @keyframes reveal
    0%
      opacity 0
      width 0px
    20%
      opacity 1
      width 0px
    30%
      width 355px
    80%
      opacity 1
    100%
      opacity 0
      width 355px
</style>
