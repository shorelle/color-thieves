<template>
  <div class="result" :class="type">
    <h3>{{ heading }}</h3>
    <button v-for="color in formattedColors" type="button" :class="color.class" :style="color.styleObject" @click="copy(color)">{{ color.label }}</button>
  </div>
</template>

<script>
  export default {
    props: [ 
      'type',
      'heading',
      'colors',
      'current'
    ],
    computed: {
      formattedColors() {
        let formattedColors = [];
        for (let i = 0; i < this.colors.length; ++i) {
          formattedColors[i] = {
            label: this.getColorValue(this.colors[i], this.current),
            styleObject: {
              'background': this.getColorValue(this.colors[i], 'RGB')
            },
            class: this.getColorClass(this.colors[i])
          }
        }
        return formattedColors;
      }
    },
    methods: {
      getColorValue(rgb, format) {
        switch(format) {
          case 'RGB':
            return 'rgb(' + rgb[0] + ',' + rgb[1] + ',' + rgb[2] + ')'
          case 'HEX':
            return this.rgbToHex(rgb)
          case 'HSL':
            return this.rgbToHsl(rgb)
        }
      },
      componentToHex(c) {
        let hex = c.toString(16)
        return hex.length === 1 ? '0' + hex : hex
      },
      rgbToHex(rgb) {
        return '#' + this.componentToHex(rgb[0]) + this.componentToHex(rgb[1]) + this.componentToHex(rgb[2])
      },
      componentPercent(c) {
        return parseInt(c * 100) + '%'
      },
      rgbToHsl(rgb) {
        let r = rgb[0] / 255, 
            g = rgb[1] / 255, 
            b = rgb[2] / 255
        let max = Math.max(r, g, b), 
            min = Math.min(r, g, b)
        let h, s, l = (max + min) / 2

        if (max === min) {
          h = s = 0; // achromatic
        } else {
          let d = max - min
          s = l > 0.5 ? d / (2 - max - min) : d / (max + min)
          switch(max) {
            case r: 
              h = (g - b) / d + (g < b ? 6 : 0)
              break
            case g: 
              h = (b - r) / d + 2
              break
            case b: 
              h = (r - g) / d + 4
              break
          }
          h /= 6
        }
        return 'hsl(' + this.componentPercent(h) + ',' + this.componentPercent(s) + ',' + this.componentPercent(l) + ')';
      },
      getColorClass(rgb) {
        return (rgb[0] * 0.299 + rgb[1] * 0.587 + rgb[2] * 0.114) > 186 ? 'light' : 'dark'
      },
      copy(color) {
        let copier = document.getElementById('copy')
        copier.value = color.label
        copier.select()
        document.execCommand('copy')
      }
    }
  }
</script>

<style lang="scss" scoped>
  @import '../assets/scss/main.scss';

  h3 {
    @include vr-font-size($h4-font-size);
    margin-bottom: 0;
    color: $midgrey;

    .empty & {
      color: $text-color;
    }
  }

  .result {
    @include clearfix();

    button {
      @include vr-padding(1 0.5);
      position: relative;
      border: 0;
      text-transform: uppercase;
      transform: scale(1);
      transition: all 0.2s ease-in-out;

      &.light {
        color: $offblack;
      }

      &.dark {
        color: $white;
      }

      &:after {
        content: '\21d2';
        position: absolute;
        opacity: 0;
        width: 0;
        margin-left: 0.25em;
        transition: all 0.1s ease-in-out;

        @include respond-to('tablet-portrait') {
          display: none;
        }
      }

      &:hover,
      &:focus {
        &:after {
          width: 1em;
          opacity: 1;
        }
      }

      &:active {
        transform: scale(1.02);
      }
    }
  }

  .dominant button {
    width: 100%;
  }

  .palette button {
    width: 50%;
    font-size: 0.75rem;
  }
</style>