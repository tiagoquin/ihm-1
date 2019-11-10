<template>
  <!--  -->
  <v-row align="start" justify="center">
    <v-col >
        <h2>Step 1: Crop your picture</h2>
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
    </v-col>
        <v-col >
          <form>
            <h2>Step 2: Get your data</h2>
            <p>
      <v-btn
        color="pink"
        class="mr-4"
        @click="getData()"
      >
      Obtain
      </v-btn>
            </p>

          <v-text-field
            label="Coordinate X"
            outlined
            :value=current.top
          ></v-text-field>
          <v-text-field
            label="Coordinate Y"
            :value=current.left
            outlined
          ></v-text-field>
          <v-text-field
            label="Width (Size X)"
            :value=current.width
            outlined
          ></v-text-field>
          <v-text-field
            label="Height (Size Y)"
            :value=current.height
            outlined
          ></v-text-field>
          <v-text-field
            label="Label"
            v-model=labelvalue
            outlined
          ></v-text-field>
          </form>

      <h2>Step 3: Add it to the list</h2>
        <p>
      <v-btn
        color="pink"
        class="mr-4"
        @click="pushData()"
      >
      Push
      </v-btn>
            </p>
        </v-col>
    <!-- List -->
    <v-col >
      <a @click.prevent="getData">getData</a>

      <v-list>
        <v-list-item-group v-model="model">
          <v-list-item v-for="elem in coordonnees" :key="elem.id">
            <v-list-item-icon>
              <v-icon large v-text="icon" @click="deleteElem(elem.id)" color="red darken-2" />
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title v-text="elem.obj.text3" />
              <v-list-item-title v-text="elem.obj.text1" />
              <v-list-item-title v-text="elem.obj.text2" />
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
      icon: "mdi-delete-forever",
      coordonnees: [],
      imgSrc: "",
      data: null,
      current: {
        top: 0,
        left: 0,
        height:0,
        width: 0,
      },
      labelvalue: '',
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
      this.current= this.data;
      // console.log(this.data);
    },
    pushData(){
      const text1 = `x: ${this.current.top}, y: ${this.current.left}`;
      const text2 = `width: ${this.current.width}, height: ${this.current.height}`;
      const text3 = `${this.labelvalue}`;
      this.coordonnees.push({ id: this.lastId++, obj: { text3, text1, text2} });
    },
    deleteElem(id){
      console.log('delete', id);
      this.coordonnees = this.coordonnees.filter(obj => obj.id !== id);
    }
  }
};
</script>