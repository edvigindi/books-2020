<template lang="pug">
  div
    vs-prompt(title='введите данные о книге' accept-text='создать' cancel-text='отменить' button-accept='border' @cancel='cancel' @accept='acceptAlert' @close='close' :is-valid="validTitle && validType && validAuthor && validDesription && validCountry && validCity" :active.sync='activeAddBookPrompt')
      div
        | Описание книги
        vs-input(placeholder='название книги' v-model='currentBook.title')
        vs-select(label='type' v-model='currentBook.type')
          vs-select-item(:key='index' :value='typeOption.value' :text='typeOption.text' v-for='typeOption, index in typeOptions')
        vs-input(placeholder='автор' v-model='currentBook.author')
        vs-input(placeholder='страна' v-model='currentBook.country')
        vs-input(placeholder='город' :disabled='!validCountry' v-model='currentBook.city')
        vs-textarea(label='описание книги' v-model='currentBook.description' counter='100' :counter-danger.sync='currentBook.counterDanger')
        .preview
          vue-cropper(ref='cropper' :aspect-ratio='1' :viewMode='3' :src='selectedImage' preview='.preview')
        image-uploader(:debug='1' :maxWidth='300' :maxHeight='300' :quality='0.9' :autoRotate='true' :preview='false' :className="['fileinput', { 'fileinput--loaded' : selectedImage }]" :capture='false' accept='image/*' doNotResize="['gif', 'svg']" @input='setImage' @onUpload='startImageResize' @onComplete='endImageResize')
          label(for='fileInput' slot='upload-label')
            figure
              svg(xmlns='http://www.w3.org/2000/svg' width='32' height='32' viewbox='0 0 32 32')
                path.path1(d='M9.5 19c0 3.59 2.91 6.5 6.5 6.5s6.5-2.91 6.5-6.5-2.91-6.5-6.5-6.5-6.5 2.91-6.5 6.5zM30 8h-7c-0.5-2-1-4-3-4h-8c-2 0-2.5 2-3 4h-7c-1.1 0-2 0.9-2 2v18c0 1.1 0.9 2 2 2h28c1.1 0 2-0.9 2-2v-18c0-1.1-0.9-2-2-2zM16 27.875c-4.902 0-8.875-3.973-8.875-8.875s3.973-8.875 8.875-8.875c4.902 0 8.875 3.973 8.875 8.875s-3.973 8.875-8.875 8.875zM30 14h-4v-2h4v2z')
            span.upload-caption {{ selectedImage ? &apos;Изменить&apos; : &apos;Выбрать&apos; }}
        // .centerx
          vs-upload(action='https://jsonplaceholder.typicode.com/posts/' @on-success='successUpload' limit=1 v-model='currentBook.image')
        vs-alert(:active='!validTitle || !validType || !validAuthor || !validCountry || !validCity' color='danger' icon='new_releases')
          | Все поля должны быть заполнены
    vs-row
      vs-col(vs-type='flex' vs-justify='center' vs-align='left' vs-w='6')
        h1 Мои предложения
      vs-col(vs-type='flex' vs-justify='center' vs-align='right' vs-w='6')
        vs-tooltip(text='Add book')
          vs-button(icon='add' size='medium' color='dark' type='flat' @click='activeAddBookPrompt = true')
    vs-row.infinite-wrapper(vs-justify='center')
      vs-col.cardsCol(:key='index' v-for='book,index in books' vs-type='flex' vs-justify='space-between' vs-lg='2' vs-sm='4' vs-xs='12')
        template
          vs-row.cardsRow(vs-type='flex' vs-justify='center' vs-align='flex-start')
            vs-col(type='flex' vs-justify='center' vs-align='center' vs-w='12')
              vs-card.cardx
                div(slot='header')
                  h3
                    | {{book.title}}
                  h4
                    | {{ book.author }}
                div(slot='media')
                  img(v-if='book.image' :src='book.image')
                  img(v-if='!book.image' src='../assets/logo.png')
                div.bookDescription
                  span
                  |   {{ book.description }}
                div.cardFooter(slot='footer')
                  vs-row
                    vs-col(vs-w='6' vs-align='flex-start')
                      vs-button(v-if='currentBook.type===1' color='dark' type='flat' icon='pan_tool' disabled)
                      vs-button(v-else color='dark' type='flat' icon='av_timer' disabled)
                    vs-col(vs-w='3' vs-align='flex-end')
                      vs-button(color='dark' type='flat' icon='edit')
                    vs-col(vs-w='3' vs-align='flex-end')
                      vs-button(color='dark' type='flat' icon='delete_sweep')
      infinite-loading(@infinite='myBooksInfiniteHandler' force-use-infinite-wrapper='.infinite-wrapper')
</template>

<script>
import VueCropper from 'vue-cropperjs'
import 'cropperjs/dist/cropper.css'
import InfiniteLoading from 'vue-infinite-loading'
export default {
  name: 'MyOffers',
  components: { VueCropper, InfiniteLoading },
  data () {
    return {
      activeAddBookPrompt: false,
      currentBook: {
        title: '',
        type: null,
        author: '',
        description: '',
        counterDanger: false,
        country: '',
        city: '',
        image: null
      },
      typeOptions: [
        {text: 'дарю', value: 1},
        {text: 'дам почитать', value: 2}
      ],
      isBooksListChanged: false
    }
  },
  computed: {
    books () {
      // источник данных о моих книгах
      return this.$store.getters.myBooks
    },
    validTitle () {
      return (this.currentBook.title.length > 0)
    },
    validType () {
      return (this.currentBook.type !== null)
    },
    validAuthor () {
      return (this.currentBook.author.length > 0)
    },
    validDesription () {
      return (this.currentBook.description.length <= 100)
    },
    validCountry () {
      return (this.currentBook.country.length > 0)
    },
    validCity () {
      return (this.currentBook.city.length > 0)
    },
    selectedImage () {
      return this.currentBook.image
    }
  },
  watch: {
    // Если изменился список
    books (newVal, oldVal) {
      this.isBooksListChanged = true
    }
  },
  methods: {
    acceptAlert (color) {
      this.$store.dispatch('newBook', {
        title: this.currentBook.title,
        author: this.currentBook.author,
        description: this.currentBook.description,
        country: this.currentBook.country,
        city: this.currentBook.city,
        type: this.currentBook.type,
        image: this.currentBook.image,
        active: ''
      })
        .then(() => {
          this.$vs.notify({
            color: 'success',
            title: 'Book Created',
            text: `Book "${this.currentBook.title}" Created`
          })
          this.submitStatus = 'OK'
          console.log(this.submitStatus)
        })
        .catch(err => {
          this.submitStatus = err.message
          console.log(this.submitStatus)
        })
      console.log(this.currentBook)
    },
    close () {
      this.$vs.notify({
        color: 'danger',
        title: 'Closed',
        text: 'You close a dialog!'
      })
    },
    cancel () {
      this.currentBook.title = ''
      this.currentBook.type = null
      this.currentBook.author = ''
      this.currentBook.description = ''
      this.currentBook.country = ''
      this.currentBook.city = ''
      this.currentBook.image = null
    },
    successUpload () {
      this.$vs.notify({
        color: 'success',
        title: 'Upload Success',
        text: 'Image downloaded'
      })
    },
    setImage (base64Image) {
      this.currentBook.image = base64Image
    },
    startImageResize () {
      console.log('startImageResize')
    },
    endImageResize () {
      console.log('endImageResize')
      // this.$refs.cropper.setCanvasData({ left: 0, top: 0, height: 220, width: 140 })
    },
    myBooksInfiniteHandler ($state) {
      this.$store.dispatch('loadMyBooks')
        .then(() => {
          console.log('isBooksListChanged', this.isBooksListChanged)
          if (this.isBooksListChanged) {
            console.log('$state.loaded()')
            $state.loaded()
          } else {
            console.log('$state.complete()')
            $state.complete()
          }
          this.isBooksListChanged = false
        })
        .catch(err => {
          console.log(err)
        })
    }
  }
}
</script>

<style scoped lang="stylus">
    div[slot="media"]
      height 300px
    img
      max-height 300px
      max-width 100%
    .preview
      max-height 200px
    input[type=file]
      width 0px
      height 0px
      position absolute
      z-index -1
      overflow hidden
      opacity 0
    .infinite-wrapper
      overflow scroll
      height 800px
    .cardsCol
      margin 20px
    .cardx
      height 370px
      padding-bottom 15px
      display grid
    .cardFooter
      align-self end
    .bookDescription
      margin auto
      height 30px
      width 135px
      word-wrap break-word
      overflow hidden
    // .vs-tooltip
      // z-index 20000
      // display block
</style>
