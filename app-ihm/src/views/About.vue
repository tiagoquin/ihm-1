<template>
  <!--  -->
  <v-row align="start" justify="center">
    <v-col cols="12" md="8">
      <v-card class="pa-2" outlined tile>
        <vue-cropper ref="cropper" :src="imgSrc" alt="Source Image" :containerStyle="style"></vue-cropper>
        <!--
        <v-img
          :src="somevalue"
          class="grey lighten-2"
          contain
          max-width="600px"
          max-height="600px"
          id="croppr"
        />
        -->
      </v-card>
    </v-col>
    <!-- List -->
    <v-col cols="6" md="4">
      <a @click.prevent="getData">getData</a>

      <v-list>
        <v-list-item-group v-model="model">
          <v-list-item v-for="elem in coordonnees" :key="elem.id">
            <v-list-item-icon>
              <v-icon v-text="icon" />
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title v-text="elem.obj.top" />
            </v-list-item-content>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-col>
  </v-row>

  <!-- button -->
</template>
<script>
import VueCropper from "vue-cropperjs";
import "cropperjs/dist/cropper.css";

export default {
  name: "about",
  data() {
    return {
      model: 1,
      icon: "mdi-wifi",
      coordonnees: [],
      imgSrc: "",
      data: null,
      lastId: 0,
      style: {
        "max-width": "500px",
        "max-height": "500px"
      }
    };
  },
  props: {
    somevalue: String
  },
  created: function() {
    this.imgSrc = this.somevalue;
  },
  mounted: function() {},
  components: { VueCropper },
  methods: {
    getData() {
      this.data = this.$refs.cropper.getCropBoxData();
      this.coordonnees.push({ id: this.lastId++, obj: this.data });
    }
  }
};
</script>