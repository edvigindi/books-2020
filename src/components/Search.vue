<template lang="pug">
  #mainDiv
    .search
      vs-input(icon='search' color='rgb(57, 74,29)' size='large')
      #parentx(ref='parentSidebar')
        vs-button(@click='active=!active' color='rgb(57, 74,29)' type='border' icon='filter_list')
        vs-sidebar.sidebarx(:parent='$refs.parentSidebar' default-index='1' color='HEX:#aba87b' spacer='' v-model='active')
          vs-sidebar-item(index='1')
            vs-input(icon='edit_location' placeholder='страна' color='HEX:#aba87b')
          vs-sidebar-item(index='2')
            vs-input(icon='streetview' placeholder='город' color='HEX:#aba87b')
          vs-sidebar-item(index='3')
            vs-button.searchButton(type='border' color='rgb(57, 74,29)') поиск
</template>

<script>
export default {
  name: 'Search',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      active: false
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
  #mainDiv
    background url('../assets/book-bg-2.jpg') center center fixed
    -webkit-background-size: cover
    -moz-background-size: cover
    -o-background-size: cover
    background-size: cover
    position: fixed
    width 100%
    height 100%
    color rgb(57, 74,29)
    font-family Helvetica, sans-serif
  .search
    width 80%
    margin auto
  .vs-input
    width 100%
  #parentx
    overflow hidden
    height 200px
    position relative
    margin-top 20px
  .search
    margin-top 20px
  .vs-con-input-label
    margin 2px
  .vs-button-border
    margin-left 2px
  .searchButton
    display block
    width 60%
    margin auto
    margin-top 10px
    padding-top 2px
    padding-bottom 2px
    height 40px
</style>
