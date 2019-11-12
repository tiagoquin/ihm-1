<template>
  <!--  -->
  <v-row
    align="start"
    justify="center"
  >
    <v-col>
      <h2>Step 1: Crop your picture</h2>
      <vue-cropper
        ref="cropper"
        :src="imgSrc"
        alt="Source Image"
        :containerStyle="style"
      ></vue-cropper>
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
        <br />
        <br />
      <v-dialog v-model="dialogb" persistent max-width="290">
      <template v-slot:activator="{ on }">
        <v-btn color="red accent-4" dark v-on="on">Change picture</v-btn>
      </template>
      <v-card>
        <v-card-title class="headline">Are you sure you want to leave?</v-card-title>
        <v-card-text>
          You'll lose everything if you didn't save.
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="red" text @click="dialogb = false">CANCEL</v-btn>
          <v-btn
            color="red accent-4"
            class="mr-4"
            to="/"
          >
          Yes, I confirm
        </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    </v-col>
    <v-col>
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
          v-model=current.top
          type=number
          @blur="setData()"
        ></v-text-field>
        <v-text-field
          label="Coordinate Y"
          v-model=current.left
          outlined
          @blur="setData()"
          type=number
        ></v-text-field>
        <v-text-field
          label="Width (Size X)"
          v-model=current.width
          outlined
          @blur="setData()"
          type="number"
        ></v-text-field>
        <v-text-field
          label="Height (Size Y)"
          v-model=current.height
          outlined
          @blur="setData()"
          type="number"
        ></v-text-field>
        <v-text-field
          label="Label"
          v-model=labelvalue
          @blur="setData()"
          outlined
        ></v-text-field>

      </form>

      <h2>Step 3: Add it to the list</h2>
      <p>
        <v-btn
          color="blue"
          class="mr-4"
          @click="pushData()"
        >
          Push
        </v-btn>
      </p>
              <v-text-field
          type="number"
          label="ID"
          v-model=labelid
          disabled
          outlined
        ></v-text-field>

    </v-col>
    <!-- List -->
    <v-col>

     <h2>Step 4: Download your list</h2>

      <v-dialog v-model="dialog" persistent max-width="290">
      <template v-slot:activator="{ on }">
        <v-btn color="success" dark v-on="on">Download</v-btn>
      </template>
      <v-card>
        <v-card-title class="headline">Name your file</v-card-title>
        <v-card-text>
          <v-text-field
          label="Filename"
          v-model=filename
          outlined
        ></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="green darken-1" text @click="dialog = false">CANCEL</v-btn>
          <v-btn
            color="green"
            class="mr-4"
            :href="thefile"
            :download="filename"
            @click="dialog = false"
          >
          Download
        </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

      <v-list>
        <v-list-item-group v-model="model">
          <v-list-item
            v-for="elem in coordonnees"
            :key="elem.id"
          >
            <v-list-item-icon>
              <v-icon
                large
                v-text="icon"
                @click="deleteElem(elem.id)"
                color="red darken-2"
              />
            </v-list-item-icon>
            <v-list-item-content @click="retrieveData(elem)">
              <span>id: {{elem.id}}</span>
              <h3>label: {{elem.label}}</h3>
              <span>x: <b>{{elem.top}}</b> y: <b>{{elem.left}}</b></span>
              <span>width: <b>{{elem.width}}</b> height: <b>{{elem.height}}</b></span>
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
  data () {
    return {
      model: 1,
      dialog: false,
      dialogb: false,
      icon: "mdi-delete-forever",
      coordonnees: [],
      ids: new Set([]),
      imgSrc: "",
      data: null,
      current: {
        top: 0,
        left: 0,
        height: 0,
        width: 0,
      },
      filename: 'filename.json',
      labelvalue: '',
      labelid: 1,
      lastId: 1,
      style: {
        "max-width": "500px",
        "max-height": "500px"
      }
    };
  },
  props: {
    somevalue: String
  },
  created: function () {
    this.imgSrc = this.somevalue;
  },
  mounted: function () { },
  components: { VueCropper },
  computed: {
    thefile() {
      // console.log(this.coordonnees);
      return `data:json/octet-stream;charset=utf-8,${JSON.stringify(this.coordonnees, null, 2)}`
    },
  },
  methods: {
    getData () {
      this.data = this.$refs.cropper.getCropBoxData();
      this.current = this.data;
      // console.log(this.data);
    },
    pushData () {

      if (this.ids.has(this.labelid)) {
        this.deleteElem(this.labelid);
      }

      this.coordonnees.push({
        id: this.labelid,
        top: this.current.top,
        left: this.current.left,
        width: this.current.width,
        height: this.current.height,
        label: this.labelvalue
      });
      this.resetVals();
    },
    setData () {
      this.$refs.cropper.setCropBoxData({
        top: Number(this.current.top), left: Number(this.current.left), width: Number(this.current.width), height: Number(this.current.height)
      });
    },
    retrieveData (obj) {
      this.current = {
        top: obj.top,
        left: obj.left,
        height: obj.height,
        width: obj.width,
      };
      this.labelvalue = obj.label;
      this.labelid = obj.id;
      this.setData();
    },
    deleteElem (id) {
      this.coordonnees = this.coordonnees.filter(obj => obj.id !== id);
    },
    resetVals () {
        this.ids.add(this.labelid);
        if(this.labelid === this.lastId) {
          ++this.lastId;
        }
        this.labelid = this.lastId;
        this.current.top = 0;
        this.current.left = 0;
        this.current.width = 0;
        this.current.height = 0;
        this.labelvalue = "";
    }
  }
};
</script>