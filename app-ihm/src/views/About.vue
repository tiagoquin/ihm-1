<template>
  <div class="about">
    <h1>This is an about page</h1>
    <img :src="somevalue" />
  </div>
</template>
<script>
export default {
  name: 'about',
  data () {
    return {
    }
  },
  props:{
    somevalue: String,
  },
  components: {
  },
  methods: {
  }
}
</script>