<template>
  <aside class="social">
    <a href="#" id="twitter" class="button twitter" target="_blank"><i class="fa fa-twitter" aria-hidden="true"></i>Tweet</a><br />
    <a href="https://github.com/shorelle/color-thieves" class="button github" target="_blank"><i class="fa fa-github" aria-hidden="true"></i>Github</a>
  </aside>
</template>

<script>
  export default {
    mounted() {
      let tweet = document.getElementById('twitter')
      tweet.href = 'https://twitter.com/share?url=' + encodeURIComponent(window.location.href) + '&text=' + encodeURIComponent(document.title)
      tweet.addEventListener('click', function(event) {
        event.preventDefault()
        const width = 600,
              height = 300,
              left = (screen.width / 2) - (width / 2),
              top = (screen.height / 2) - (height / 2)
        window.open(
          tweet.href,
          'tweetWindow',
          'toolbar=no, location=no, directories=no, status=no, menubar=no, scrollbars=no, resizable=no, copyhistory=no, width=' + width + ', height=' + height + ', top=' + top + ', left=' + left
        )
      }, false)
    }
  }
</script>

<style lang="scss" scoped>
  @import '../assets/scss/main.scss';

  aside.social {
    position: absolute;
    top: 1rem;
    right: 1rem;
    text-align: right;
  }

  a.button {
    @include vr-margin(0 0 $mod-xxs);
    width: 140px;
    padding-left: 0;
    padding-right: 0;
    text-align: center;
    font-size: 0.9rem;

    i {
      margin-right: 0.4rem;
    }
  }

  $twitter: #1b95e0;
  $twitter-light: #abd4ee;

  a.button.twitter {
    border-color: $twitter-light;
    color: $twitter;

    &:hover,
    &:focus {
      background: $twitter-light;
      color: darken($twitter, 5%);
    }
  }

  $github: #6e5494;
  $github-light: #b0a3c4;

  a.button.github {
    border-color: $github-light;
    color: $github;

    &:hover,
    &:focus {
      background: $github-light;
      color: darken($github, 5%);
    }
  }
</style>