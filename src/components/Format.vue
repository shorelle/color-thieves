<template>
  <div class="formats">
    <button v-for="format in formats" type="button" :class="isActive(format)" @click="select(format)">{{ format }}</button>
  </div>
</template>

<script>
  import bus from '../bus'

  export default {
    props: [
      'current'
    ],
    data() {
      return {
        formats: [
          'HEX',
          'RGB',
          'HSL'
        ]
      }
    },
    methods: {
      isActive(format) {
        return {
          active: this.current === format
        }
      },
      select(format) {
        bus.$emit('selectFormat', format)
      }
    }
  }
</script>

<style lang="scss" scoped>
  @import '../assets/scss/main.scss';

  .formats {
    @include clearfix();

    button {
      width: 32%;
      margin-right: 2%;

      &:nth-of-type(3n) {
        margin-right: 0;
      }
    }
  }
</style>