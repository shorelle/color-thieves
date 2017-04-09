<!-- Modified from Vue Dropzone wrapper by rowanwins
https://github.com/rowanwins/vue-dropzone -->
<template>
  <form :action="url" :id="id" class="vue-dropzone dropzone"></form>
</template>

<script>
  export default {
    props: [
      'id',
      'url'
    ],
    data() {
      return {
        dropzoneOptions: {
          acceptedFiles: 'image/*',
          dictDefaultMessage: 'Drop an image<br>or click to upload',
          thumbnailWidth: null,
          thumbnailHeight: null,
          previewTemplate: '<div class="dz-preview dz-file-preview"><div class="dz-image"><img data-dz-thumbnail /></div><div class="dz-progress"><span>Analyzing</span><span class="loading"></span></div><div class="dz-error-message"><span data-dz-errormessage></span></div></div>',
          init: function() {
            // remove previous file on each upload
            this.on('addedfile', function() {
              if (this.files[1] != null) {
                this.removeFile(this.files[0])
              }
            });
          }
        }
      }
    },
    methods: {
      setOption: function (option, value) {
        this.dropzone.options[option] = value
      },
      removeAllFiles: function () {
        this.dropzone.removeAllFiles(true)
      },
      processQueue: function () {
        this.dropzone.processQueue()
      },
      removeFile: function (file) {
        this.dropzone.removeFile(file)
      }
    },
    mounted() {
      if (this.$isServer) return
      var Dropzone = window.Dropzone
      Dropzone.autoDiscover = false
      var element = document.getElementById(this.id)
      this.dropzone = new Dropzone(element, this.dropzoneOptions)

      // Handle the dropzone events
      var vm = this

      this.dropzone.on('thumbnail', function (file) {
        vm.$emit('vdropzone-thumbnail', file)
      })

      this.dropzone.on('addedfile', function (file) {
        vm.$emit('vdropzone-file-added', file)
      })

      this.dropzone.on('removedfile', function (file) {
        vm.$emit('vdropzone-removed-file', file)
      })

      this.dropzone.on('success', function (file, response) {
        vm.$emit('vdropzone-success', file, response)
      })

      this.dropzone.on('successmultiple', function (file, response) {
        vm.$emit('vdropzone-success-multiple', file, response)
      })

      this.dropzone.on('error', function (file, error, xhr) {
        vm.$emit('vdropzone-error', file, error, xhr)
      })

      this.dropzone.on('sending', function(file, xhr, formData){
        vm.$emit('vdropzone-sending', file, xhr, formData)
      })

      this.dropzone.on('sendingmultiple', function(file, xhr, formData){
        vm.$emit('vdropzone-sending-multiple', file, xhr, formData)
      })

      this.dropzone.on('queuecomplete', function(file, xhr, formData){
        vm.$emit('vdropzone-queue-complete', file, xhr, formData)
      })
    },

    beforeDestroy() {
      this.dropzone.destroy()
    }
  }

</script>

<style lang="scss">
  @import '../assets/scss/main.scss';

  .dropzone {
    @include vr-font-size($h3-font-size);
    @include vr-line-height($h4-font-size);
    position: relative;
    float: left;
    width: 100%;
    background: $lightgrey;
    border: 3px darken($lightgrey, 10%) dashed;
    color: darken($text-color, 10%);
    text-align: center;

    &:hover {
      background: lighten($lightgrey, 5%);
      border-color: $white;
      color: darken($text-color, 5%);
      cursor: pointer;
    }

    img {
      float: left;
      width: 100%;
      height: auto;
    }

    &.dz-started {
      background: none;
      border-color: $white;

      .dz-message { display: none; }
    }
  }

  .dz-message {
    @include vr-padding($mod-xl 1);

    @include respond-to('mobile') {
       @include vr-padding($mod-l 1);
    }
  }

  .dz-preview {
    position: relative;
    float: left;
    width: 100%;
    background: $lightgrey;
    min-height: $base-line-height * $mod-l * 6;
  }

  .dz-image {
    transition: opacity 0.2s ease-in-out;

    .dz-processing & {
      opacity: 0.5;
    }

    .dz-complete & {
      opacity: 1;
    }
  }

  .dz-progress {
    position: absolute;
    top: 50%;
    left: 0;
    margin-top: -$base-line-height;
    width: 100%;
    text-align: center;
    color: $white;
    opacity: 1;
    transition: opacity 0.2s ease-in-out;

    span { 
      display: block;
      animation: blink 1.5s linear infinite;
    }

    .dz-complete & {
      opacity: 0;
    }
  }

  .loading:after {
    content: '\2026';
    display: inline-block;
    overflow: hidden;
    vertical-align: bottom;
    font-family: $heading-font;
    animation: ellipsis steps(4, end) 900ms infinite;
    width: 0;
  }

  @keyframes blink {
    50% { opacity: 0.2; }
  }

  @keyframes ellipsis {
    to {
      width: 1.1em;
    }
  }
</style>