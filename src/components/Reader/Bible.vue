<template>
  <div class="reader">
    <div class="ss-wrapper">
      <div :class="{
            'ss-wrapper-andada': readerOptionsStore().font === READER_FONT.ANDADA,
            'ss-wrapper-lato': readerOptionsStore().font === READER_FONT.LATO,
            'ss-wrapper-pt-serif': readerOptionsStore().font === READER_FONT.PTSERIF,
            'ss-wrapper-pt-sans': readerOptionsStore().font === READER_FONT.PTSANS,
            }"
           class="ss-wrapper-font">
        <div :class="{
              'ss-wrapper-tiny': readerOptionsStore().size === READER_SIZE.XS,
              'ss-wrapper-small': readerOptionsStore().size === READER_SIZE.SM,
              'ss-wrapper-medium': readerOptionsStore().size === READER_SIZE.MD,
              'ss-wrapper-large': readerOptionsStore().size === READER_SIZE.LG,
              'ss-wrapper-huge': readerOptionsStore().size === READER_SIZE.XL,
            }"
             class="ss-wrapper-size">
          <div :class="{
            'ss-wrapper-light': readerOptionsStore().theme === READER_THEME.LIGHT || (readerOptionsStore().theme === READER_THEME.AUTO && !darkMode),
            'ss-wrapper-sepia': readerOptionsStore().theme === READER_THEME.SEPIA,
            'ss-wrapper-dark': readerOptionsStore().theme === READER_THEME.DARK || (readerOptionsStore().theme === READER_THEME.AUTO && darkMode),
          }" class="ss-wrapper-theme p-5">
            <div :dir="dir" :class="[dir === 'rtl' ? 'text-right' : '']" v-html="content" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { READER_THEME, READER_FONT, READER_SIZE, readerOptionsStore } from '@/components/Reader/ReaderOptionsStore'
import { useLanguageStore } from '@/stores/language'
import { storeToRefs } from 'pinia'
import { watch } from 'vue'

export default {
  data () {
    return {
      READER_THEME, READER_FONT, READER_SIZE, readerOptionsStore,
      dir: 'ltr'
    }
  },
  mounted () {
    const languageStore = storeToRefs(useLanguageStore())
    watch(languageStore, this.directionCalc)
    this.directionCalc()
  },
  beforeUnmount () {
    const languageStore = storeToRefs(useLanguageStore())

  },
  methods: {
    directionCalc: function () {
      let languageCode = useLanguageStore().locale.code
      this.dir = ['ar', 'fa', 'he'].includes(languageCode) ? 'rtl' : 'ltr'
    }
  },
  computed: {
    darkMode: function () {
      return window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches
    },
  },
  props: {
    content: String
  }
}
</script>