<template>
        <v-row
          align="center"
          justify="space-around"
        >
      <v-col
            cols="12"
            sm="8"
            md="4"
          >
    <Upload />
      </v-col>
        </v-row>
</template>

<script>
// @ is an alias to /src
import Upload from '@/components/Upload.vue'

export default {
  name: 'home',
  components: {
    Upload
  }
}
</script>
