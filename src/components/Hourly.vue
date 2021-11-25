<template>
    <v-main>
        <v-card height="40vh" class="ma-3 mb-6" >
            <h1 class="display-1 pa-6" >Next Hours</h1>

            <v-sheet class="mx-0" max-width="100%" justify="center">
              <v-slide-group v-model="model" light>
                  <v-slide-item v-for="(n,k,i) in weatherData" touch :key="n"  >
                    <v-col>
                        <v-row >
                            <v-col cols="auto" >
                                <v-dialog
                                  transition="dialog-top-transition"
                                  max-width="600"
                                  width="100vw"
                                    >
                                    <template v-slot:activator="{ on, attrs }"  >
                                        <v-card class="ma-0"  elevation="0"  v-bind="attrs" v-on="on" >
                                            <p class="text-center">{{Math.round(weatherData.hourly[i].temp)}}°</p>
                                            <p class="text-center grey--text" >{{dateHourlyFormatter(weatherData.hourly[i].dt)}}</p>
                                            <v-img :src="`http://openweathermap.org/img/wn/${weatherData.hourly[i].weather[0].icon}@2x.png`" height="10vh" width="18vw" contain  ></v-img>
                                            <p class="text-center blue--text">{{weatherData.hourly[i].humidity}}%</p>
                                        </v-card>
                                    </template>
                                    
                                    <template v-slot:default="dialog">
                                        <v-card align="center">
                                            <v-toolbar
                                              color="#010600"
                                              dark
                                              clas="display-2"
                                            >Time: {{dateHourlyFormatter(weatherData.hourly[i].dt)}}</v-toolbar>
                                            <v-card-text class="pt-6">
                                                <p class="text-center title grey--text">Temperature:</p>
                                                <p class="text-center title">{{Math.round(weatherData.hourly[i].temp)}}°</p>
                                                <p class="text-center blue--text">Humidity: {{weatherData.hourly[i].humidity}}%</p>
                                                <v-img
                                                :src="`http://openweathermap.org/img/wn/${weatherData.hourly[i].weather[0].icon}@2x.png`"
                                                height="20vh"
                                                width="30vw"
                                                contain
                                                @click="togglePopup = !togglePopup"
                                                v-bind="attrs"
                                                v-on="on"
                                                ></v-img>
                                                <p class="text-center grey--text">{{weatherData.hourly[i].weather[0].main}}</p>
                                            </v-card-text>
                                            <v-card-actions class="justify-end">
                                                <v-btn
                                                  text
                                                  @click="dialog.value = false"
                                                >Close</v-btn>
                                            </v-card-actions>
                                        </v-card>
                                    </template>
                                </v-dialog>
                            </v-col>
                        </v-row>
                    </v-col>
                  </v-slide-item>
              </v-slide-group>
            </v-sheet>
        </v-card>
    </v-main>
</template>

<script>
export default {
    props:['weatherData'],
    data:() =>({
        model: 'tab',
    }),
    methods:{
        dateHourlyFormatter (unixTime){
          let d = new Date(unixTime * 1000);

          let hours = d.getHours();
          var pmam = '';

          if(hours >= 12)
            pmam = ' PM'
          else
            pmam = ' AM'

          return `${hours}`+ ":00" + pmam;
        },
        fun(test){
            console.log(test);
        }
    }

}
</script>

<style>

</style>