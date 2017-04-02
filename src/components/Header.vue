<template>
  <header>
    <h1>colorthieves</h1>
    <div class="subtitle">
      <span class="highlight">//</span> drag&amp;drop image to find color palette<br />
      <span class="highlight">//</span> uses <a href="https://github.com/lokesh/color-thief" target="_blank">color-thief</a> module
    </div>
  </header>
</template>

<script>
  export default {}
</script>

<style lang="scss" scoped>
  @import '../assets/scss/main.scss';

  header {
    @include columns(12);
    @include vr-margin($mod-l 0 0);
  }

  h1 {
    display: inline-block;
    margin: 0;
    font-family: $heading-font;
    text-shadow: -4px 0 0 $white;
  }

  .subtitle {
    display: inline-block;
  }
</style>
