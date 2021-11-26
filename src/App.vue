<template>
  <v-app >
    <v-main >
      <v-toolbar
        color="blue darken-2"
        dark
        flat
        >
        <v-toolbar-title >Simple Weather</v-toolbar-title>
        <v-spacer ></v-spacer>
        <v-spacer></v-spacer>
        <v-text-field
          label="Enter city..."
          hide-details="auto"
          v-show="showInput"
          width="100px"
          v-model="query"
          ></v-text-field>
          <v-spacer ></v-spacer>
          <v-btn light color="amber" v-show="showInput" small @click="formValidation(query)">Search</v-btn>
        <v-btn icon>
          <v-icon @click="showInput = !showInput">mdi-magnify</v-icon>
        </v-btn> 
        <template v-slot:extension>
          <v-tabs
            v-model="model"
            left
            slider-color="deep-orange accent-4"
            background-color="white"
            color="black"
            light
            >
            <v-tab
              v-for="i in cities"
              class="tab"
              :key="i.number"
              :href="`#tab-${i.number}`"
              @click="fetchWeather(i.lat,i.lon)"
              >
              {{ i.name }}
            </v-tab>
          </v-tabs>
        </template>
      </v-toolbar> 
      <v-tabs-items v-model="model" touchless >
        <v-tab-item
          v-for="i in cities"
          :key="i"
          :value="`tab-${i.number}`"
          >
          <v-container flat class="blue" >
            <Hourly :weather-data="weather" />
            <Daily :weather-data="weather" />
          </v-container>
        </v-tab-item>
      </v-tabs-items>
    </v-main>
  </v-app>
</template>
<script>

import Daily from './components/Daily.vue';
import Hourly from './components/Hourly.vue';
import citiesList from './cities_20000.json'

export default {
  components: { Hourly, Daily },
  name: 'App',
  data: () => ({
    api_key: '1f89da47fe4d0be6bbbf376af70bdb58',
    url_base: 'https://api.openweathermap.org/data/2.5/onecall',
    weather: {},
    showInput:false,
    model: 'tab',
    citiesList,
    query:'',
    cities:[
      {
        number:1,
        name:"RIO DE JANEIRO",
        lat:'-22.0',
        lon:'-42.5'
      },
      {
        number:2,
        name:"BEIJING",
        lon: "116.397232",
        lat: "39.907501"
      },
      {
        number:3,
        name:"LOS ANGELES",
        lon: "-118.243683",
        lat: "34.052231"
      },
    ],
  }),
  created(){
    this.fetchWeather('-22.0','-42.5')
  },
  methods:{
      async fetchWeather (lat,lon){
        await fetch(`${this.url_base}?lat=${lat}&lon=${lon}&units=metric&exclude=minutely&appid=${this.api_key}`)
        .then(res =>{
          return res.json();
        }).then(this.setResults);
      },
      setResults (results){
        this.weather = results;
      },
      formValidation(query){
          var formattedQuery = query.toLowerCase()
          var counter=0
          for(var i in this.citiesList){
            var formattedString = this.citiesList[i].city_name.toLowerCase()
            if(formattedQuery == formattedString.trim()){
              this.fetchWeather(this.citiesList[i].lat,this.citiesList[i].lon)
              break
            }
            else{
              counter++
            }
          }
          if(counter == citiesList.length){
          alert("City doesn't exist in data or is mistyped!")
          }
      }
    }
  }
</script>
<style scoped>
</style>
