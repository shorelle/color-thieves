<template>
  <div id="app">
    <div class="container">
      <AppHeader />
      <main>
        <div class="image-drop">
          <Dropzone id="dropzone" url="https://httpbin.org/post" @vdropzone-success="success" />
        </div>
        <div class="image-results">
          <Format :current="format" />
          <Palette v-if="showResult" type="dominant" heading="Dominant Color" :current="format" :colors="dominantColor" />
          <Palette v-if="showResult" type="palette" heading="Palette" :current="format" :colors="paletteColors" />
          <Palette v-else="showResult" type="empty" heading="↑ select color mode" :colors="{}" />
          <input id="copy" class="hidden" type="text" value="">
        </div>
        <Social />
      </main>
      <AppFooter />
    </div>
  </div>
</template>

<script>
  import bus from './bus'
  import AppHeader from './components/Header'
  import Dropzone from './components/Dropzone'
  import Format from './components/Format'
  import Palette from './components/Palette'
  import Social from './components/Social'
  import AppFooter from './components/Footer'

  const colorThief = new ColorThief()

  export default {
    name: 'app',
    components: { 
      AppHeader, 
      Dropzone, 
      Format, 
      Palette, 
      Social, 
      AppFooter
    },
    data() {
      return {
        format: 'HEX',
        showResult: false,
        dominantColor: [],
        paletteColors: []
      }
    },
    created() {
      bus.$on('selectFormat', format => {
        this.format = format;
      })
    },
    methods: {
      success(file) {
        let image = document.getElementsByTagName('img')[0]
        this.dominantColor = [colorThief.getColor(image)]
        this.paletteColors = colorThief.getPalette(image, 6)
        this.showResult = true
      }
    }
  }
</script>

<style lang="scss">
  @import './assets/scss/main.scss';

  main {
    @include vr-margin($mod-l 0 $mod-xl);
    @include columns(12, 0, 0);

    @include respond-to('mobile') {
      @include vr-margin(1 0);
    }
  }

  .image-drop {
    @include columns(7);
    @include responsive-columns('tablet-landscape', 6);
    @include responsive-columns('mobile', 1);

    @include respond-to('mobile') {
      @include vr-margin(0 5% $mod-s);
    }
  }

  .image-results {
    @include columns(4.5, 0.5);
    @include responsive-columns('tablet-landscape', 6);
    @include responsive-columns('mobile', 1);
  }
</style>