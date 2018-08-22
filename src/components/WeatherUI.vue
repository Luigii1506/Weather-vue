<template>
  <v-container fluid class="wrapper">
    <v-layout v-bind="binding" wrap>
      <v-flex xs12 sm3 md3 lg2 class="padding20" v-for="weather in array">
        <v-card class="v-card">
         <v-card-text>
          <p class="title-card font-weight-regular">{{weather.ciudad}} </p>
         </v-card-text>
         <div style="image-div">
           <img v-bind:src="weather.url" class="image" width="256" height="256">
         </div>
         <v-card-text>
           <p class="temp-card display-2">{{weather.temp}} </p>
           <p class="title-card">{{weather.weather}} </p>
         </v-card-text>
         <v-card-title>
           <v-layout column class="layout-min-max">
              <div id="triangle-down" class="triangle"></div>
              <span>{{weather.min_temp}}</span>
              <p class="min-value">Min</p>
            </v-layout>
            <v-layout column class="layout-min-max">
              <div id="triangle-up" class="triangle"></div>
              <span>{{weather.max_temp}}</span>
              <p class="max-value">Max</p>
            </v-layout>
         </v-card-title>
        </v-card>
      </v-flex>
      <v-btn fab fixed bottom right :color="theme ? 'red': 'indigo'" :dark="!theme" @click.stop="dialog = true">
        <v-icon color="white">add</v-icon>
      </v-btn>
    </v-layout>
    <v-dialog
      v-model="dialog"
      max-width="300"
    >
      <v-card>
        <v-card-title class="headline image">Search city</v-card-title>
        <v-card-text>
          <v-text-field
            v-model="city"
            label="City"
            box
          ></v-text-field>
        </v-card-text>
        <v-card-actions>

          <v-spacer></v-spacer>
          <v-btn
            color="green darken-1"
            flat="flat"
            @click="addCity()"
          >
            Add city
          </v-btn>
          <v-btn
            color="green darken-1"
            flat="flat"
            @click="dialog = false"
          >
            Close
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>
<script>
export default {
  data() {
    return {
      array: [
      {ciudad: 'Tijuana', max_temp: '50', min_temp: '20', temp: '30°', weather: 'sunny', url: require('../assets/sun.png')},
      {ciudad: 'Honolulu', max_temp: '20', min_temp: '3', temp: '10°', weather: 'rain', url: require('../assets/raining.png')},
      {ciudad: 'Tecate', max_temp: '20', min_temp: '5', temp: '15°', weather: 'partially cloudy', url: require('../assets/cloudy.png')},
      {ciudad: 'Osaka', max_temp: '40', min_temp: '20', temp: '30°', weather: 'sunny', url: require('../assets/sun.png')}

    ],
    info: null,
    imagen: null,
    city: '',
    dialog: false
    }
  },

  name: 'WeatherUI',
  props: ['theme'],
  methods: {
    addCity() {
      axios.get('http://api.apixu.com/v1/forecast.json?key=f064fc75bc2a4fe69ff213458182208&q=' + this.city).then(response => (this.info = response)).then(response=> (this.createObject()))
      this.dialog = false
    },
    // Funcion para limpiar los datos que recibo de la API para posteriormente pushearlos al array
    createObject() {

      // Validacion para elegir que imagen se mostrara
      if(this.info.data.current.condition.text == 'Sunny')
        this.imagen = require('../assets/sun.png')
      else if(this.info.data.current.condition.text == 'Partly cloudy')
        this.imagen = require('../assets/cloudy.png')
      else if(this.info.data.current.condition.text.includes("rain"))
        this.imagen = require('../assets/raining.png')
      else
        this.imagen = require('../assets/cloudy.png')

      // Creo el objeto
      let object = {
        ciudad: this.info.data.location.name,
        max_temp: this.info.data.forecast.forecastday[0].day.maxtemp_c,
        min_temp: this.info.data.forecast.forecastday[0].day.mintemp_c,
        temp: this.info.data.current.temp_c+'°',
        weather: this.info.data.current.condition.text,
        url: this.imagen
      }
      console.log(this.info)
      // Empujo los datos
      this.array.push(object)

    }
  },
  computed: {
    // Funcion para cambiar el v-layout entre column y row
      binding () {
        const binding = {}
        if (!this.$vuetify.breakpoint.mdAndUp) binding.column = true
        return binding
      }
    }
}
</script>

<style scoped>

.search {
  width: 100%;
}

.image-div {
  width: 100%;
}

.image {
  margin-left: auto;
  margin-right: auto;
  display: block;
  width: 100%;
  height: auto;
  max-width: 230px;
  max-height: 230px;
}

.padding20 {
  margin-left: 4%;
  margin-right: 4%;
  margin-bottom: 30px;
}

.wrapper {
  margin-top: 20px;
}

.title-card {
  text-align: center;
  margin: 0;
  font-size: 25px;
  margin-right: 7px;
}

.temp-card {
  font-size: 40px;
  text-align: center;
  margin: 0;
}

.test {
  position: relative;
}

.v-card {
  border-radius: 15px;
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
  transition: 0.3s;
}

.v-card:hover {
    box-shadow: 0 8px 16px 0 rgba( 0, 0, 0, 0.8 );
}

span {
  font-size: 20px;
}

#triangle-down {
	width: 0;
	height: 0;
	border-left: 10px solid transparent;
	border-right: 10px solid transparent;
	border-top: 12px solid #62FF91;
}

#triangle-up {
	width: 0;
	height: 0;
	border-left: 10px solid transparent;
	border-right: 10px solid transparent;
	border-bottom: 12px solid #F02102;
}

.layout-min-max {
  text-align: center;
}

.triangle {
  margin-left: auto;
  margin-right: auto;
}

.min-value {
  font-size: 16px;
  color: #62FF91;
}

.max-value {
  font-size: 16px;
  color: #F02102;
}

</style>
