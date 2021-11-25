<template>
  <v-app class="blue">
    <v-main >
      <v-toolbar
        color="#1565c0"
        dark
        flat
        >
        <v-toolbar-title>Simple Weather</v-toolbar-title>
        <v-spacer></v-spacer>

        <v-btn icon>
          <v-icon>mdi-magnify</v-icon>
        </v-btn> 
        <template v-slot:extension>
          <v-tabs
            v-model="model"
            left
            slider-color="#f84b23"
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

export default {
  components: { Hourly, Daily },
  name: 'App',
  data: () => ({
    api_key: '1f89da47fe4d0be6bbbf376af70bdb58',
    url_base: 'https://api.openweathermap.org/data/2.5/onecall',
    weather: {},
    model: 'tab',
    text: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.',
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
  }
};
</script>
<style scoped>
</style>