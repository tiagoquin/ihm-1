<template>
 <v-app id="inspire">
    <v-content>
      <v-container
        fluid
      >

    <router-view/>
      </v-container>
                 <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link :to="{ name: 'about', params: {somevalue: 'chien' } }">About</router-link>
    </div>
    </v-content>
  </v-app>
</template>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
