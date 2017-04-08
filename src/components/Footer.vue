<template>
  <footer>
    <p><span class="highlight">//</span> a <a href="https://vuejs.org" target="_blank">vue.js</a> project by <a href="http://shorelle.net" target="_blank">shorelle.net</a></p>
  </footer>
</template>

<script>
  export default {}
</script>

<style lang="scss" scoped>
  @import '../assets/scss/main.scss';

  footer {
    @include columns(12);
    @include vr-margin(0 0 $mod-l);
    border-top: 1px solid $lightgrey;

    @include respond-to('mobile') {
      @include vr-margin(0 5% $mod-s);
    }
  }
</style>
