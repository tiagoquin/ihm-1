<template>
  <v-card class="elevation-12">
    <v-toolbar
      color="primary"
      dark
      flat
    >
      <v-toolbar-title>Load your picture</v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>
    <v-card-text>
      <v-form>
        <picture-input
          ref="pictureInput"
          width="600"
          height="600"
          margin="16"
          accept="image/jpeg,image/png"
          size="10"
          button-class="btn"
          :custom-strings="{
        upload: '<h1>Bummer!</h1>',
        drag: 'Drag and drop your picture here! :D'
      }"
          @change="onChange"
        >
        </picture-input>
      </v-form>
    </v-card-text>
    <v-card-actions>
      <v-spacer></v-spacer>
      <v-btn
        color="primary"
        :disabled="ready"
        :to="{ name: 'about', params: {somevalue: this.url } }"
      >
        Next
      </v-btn>

    </v-card-actions>
  </v-card>
</template>

<script>
import PictureInput from 'vue-picture-input'

export default {
  name: 'upload',
  components: {
    PictureInput,
  },
  data: function () {
    return {
      ready: true,
      url: 'nothing',
    }
  },
  props: {
    source: String,
  },
  methods: {
    onChange (image) {
      console.log('New picture selected!')
      if (image) {
        console.log('Picture loaded.')
        this.image = image
        this.url = image;
        this.ready = false;
        console.log(image);
      } else {
        console.log('FileReader API not supported: use the <form>!')
      }
    }
  }
}
</script>