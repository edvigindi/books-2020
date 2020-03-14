<template lang="pug">
  #mainDiv
    .contacts
        vs-input(label='тема письма' color='HEX:#394a1d')
        vs-input(type='email' label='e-mail' color='HEX:#394a1d')
        vs-textarea(label='содержание' color='HEX:#394a1d')
        vs-button(type='border' color='rgb(57, 74,29)') отправить
</template>

<script>
export default {
  name: 'Contacts',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  computed: {
    cmp1 () {
      return 'Test'
    }
  }
}
</script>

<style scoped lang="stylus">
  html, body, #mainDiv
    overflow auto
  #mainDiv
    background url('../assets/book-bg-2.jpg') center center fixed
    -webkit-background-size: cover
    -moz-background-size: cover
    -o-background-size: cover
    background-size: cover
    position: fixed
    width 100%
    height 100%
  .contacts
    background-color #ffffff
    margin auto
    margin-top 20px
    padding-top 20px
    padding-bottom 20px
    width 30%
    max-height 800px
    opacity .6
  .vs-component
    height 60px
    margin auto
    width 60%
  .vs-con-textarea
    margin-top 10px
  .vs-button
    display block
    width 60%
    margin auto
    margin-top 10px
    padding-top 2px
    padding-bottom 2px
    height 40px
</style>
