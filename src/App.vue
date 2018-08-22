<template>
  <v-app :dark="dark">
    <v-toolbar
      app
      :clipped-left="clipped"
    >
      <v-layout row wrap>
        <v-flex xs9 sm10 md2 lg2>
          <div class="menu-toolbar">
            <v-toolbar-side-icon></v-toolbar-side-icon>
            <v-toolbar-title v-text="title" class="text"></v-toolbar-title>
          </div>
        </v-flex>
        <v-flex sm8 lg8 md8 class="hidden-xs-only hidden-sm-only">
          <v-toolbar-title v-text="tittleCenter" class="text-center "></v-toolbar-title>
        </v-flex>
        <v-flex xs3 sm2 md2 lg2>
          <v-switch label="Dark" v-model="dark" class="switch"></v-switch>
        </v-flex>
      </v-layout>
    </v-toolbar>
    <v-content>
      <WeatherUI :theme="dark"/>
    </v-content>
  </v-app>
</template>

<script>
import WeatherUI from './components/WeatherUI'

export default {
  name: 'App',
  components: {
    WeatherUI
  },
  data () {
    return {
      clipped: false,
      drawer: true,
      fixed: false,
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Minimis',
      tittleCenter: 'TODAY',
      dark: false
    }
  }
}
</script>

<style scoped>

.switch {
  height: 25px;
  margin-top: 10px;
}
.text {
  display: inline;
  line-height: 50px;
}

.text-center {
  text-align: center;
  line-height: 50px;
}

</style>
