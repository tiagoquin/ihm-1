<template>
  <!--  -->
  <v-row
    align="start"
    justify="center"
  >
    <v-col
      cols="12"
      md="8"
    >
      <v-card
        class="pa-2"
        outlined
        tile
      >
        <v-img
          :src="somevalue"
          class="grey lighten-2"
          contain
          max-width="600px"
          max-height="600px"
          id="croppr"
        />
      </v-card>
    </v-col>
    <v-col
      cols="6"
      md="4"
    >
      <v-list>
        <v-list-item-group v-model="model">
          <v-list-item
            v-for="elem in coordonnees"
            :key="elem"
          >
            <v-list-item-icon>
              <v-icon v-text="icon" />
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title v-text="elem" />
            </v-list-item-content>

          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-col>
  </v-row>
</template>
<script>
const Croppr = require('croppr');

export default {

  name: 'about',
  data () {
    return {
      model: 1,
      icon: 'mdi-wifi',
      coordonnees: [
        'truc',
        'bidule'
      ],
      croppr: null,
    }
  },
  props: {
    somevalue: String,
  },
  mounted: function () {
    this.croppr = new Croppr('#croppr', {});
  },
  components: {
  },
  methods: {
  }
}
</script>