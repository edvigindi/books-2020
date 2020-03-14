<template lang="pug">
  .footer
    .contacts
      vs-divider(color='#ffffff' position='right-center') КОНТАКТЫ
    .social
      i.fas.fa-telegram-plane
</template>
<script>
export default {
  name: 'Footer',
  data () {
    return {
    }
  }
}
</script>
<style scoped lang="stylus">
  .footer
    font-family Helvetica, sans-serif
    position fixed
    bottom 0
    height 100px
    width 100%
    background-color #394a1d
    color #ffffff
</style>
