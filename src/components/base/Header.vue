<template lang="pug">
    vs-navbar.nabarx(
      v-model='activeItem'
      type="border"
      color="#aba87b"
      text-color="#ffffff"
      )
      div(slot='title')
        vs-navbar-title
          img.logoImg(src='../../assets/logo.png')
      vs-navbar-item.item-main(
        v-for="(link, index) in linkMenu"
        :key="link.title"
        @click="menuShow = false"
        :index="index"
        color="#aba87b"
      )
        router-link.navbar-link(
          :to="`${link.url}`"
        ) {{ link.title }}
      // Статическое формирование пункта меню "Выйти"
      vs-navbar-item(
        v-if="checkUser"
      )
        .examplex
          vs-dropdown
            a.a-icon(href.prevent='')
              img(:src="userData.photo" style="height: 32px; width: 32px; border-radius: 50%")
              vs-icon(icon='expand_more')
            vs-dropdown-menu
              vs-dropdown-item(disabled='')
                |{{userData.name}}
              vs-dropdown-item(disabled='')
                |{{userData.email}}
              vs-dropdown-item(divider='')
                span.navbar-link(@click='signOut') SignOut
</template>
<script>
import firebase from 'firebase'
import store from '../../store'
export default {
  name: 'Header',
  data () {
    return {
      menuShow: false,
      activeItem: 0
    }
  },
  created () {
    // Обработчик событий "пользователь вошел / вышел"
    firebase.auth().onAuthStateChanged(function (user) {
      if (user) {
        store.dispatch('loggedUser', user)
      } else {}
    })
  },
  computed: {
    checkUser () {
      return store.getters.checkUser
    },
    userData () {
      return store.getters.user
    },
    isLoading () {
      return this.$store.getters.loading
    },
    linkMenu () {
      return (this.checkUser) ? [
        { title: 'главная', url: '/', icon: 'mdi-home' },
        { title: 'поиск книги', url: '/search', icon: 'mdi-book-search-outline' },
        { title: 'мои предложения', url: '/my-offers', icon: 'mdi-format-list-bulleted' },
        { title: 'контакты', url: '/contacts', icon: 'mdi-contact-phone-outline' }
      ] : [
        { title: 'главная', url: '/', icon: 'mdi-home' },
        { title: 'контакты', url: '/contacts', icon: 'mdi-contact-phone-outline' }
      ]
    }
  },
  // Наблюдение за значением route
  watch: {
    $route (to, from) {
      // Если нет текущего пользователя
      // и текущий роут не "Home" || "Contacts"
      if (!this.checkUser && to.name !== 'Home' && to.name !== 'Contacts') {
        // Переадресуем пользователя на раздел "Home"
        this.$router.push('/')
      }
      // Иначе переадресуем на желаемый раздел сайта
    }
  },
  methods: {
    signOut () {
      // Вызываем выход из учетной записи в текущем приложении
      store.dispatch('logoutUser')
        .then(() => {
          this.$router.push('/search')
        })
    }
  }
}
</script>
<style scoped lang="stylus">
  .is-active-item
    font-weight bold
    font-family Helvetica, sans-serif
    border-bottom solid 1px white
  .navbar-link
    color #ffffff
    padding 10px 15px
    text-transform uppercase
    font-family Helvetica, sans-serif
    font-weight lighter
  .navbar-link:hover
    transition .2s
    border-bottom solid 1px white
    background-color #b8b69a
  .vs-navbar--title
    color #ffffff
    display flex
  .logoImg
    height 80px
    margin 10px
</style>
