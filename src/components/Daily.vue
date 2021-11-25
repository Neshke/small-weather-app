<template>
    <v-main>
        <v-card class="ma-3">
            <h1 class="display-1 pa-6">Next 5 days</h1>
            <div v-for="(n,k,i) in weatherData" :key="k" class="d-flex pa-4">
                <v-img :src="`http://openweathermap.org/img/wn/${weatherData.daily[i].weather[0].icon}@2x.png`" height="10vh" width="15vw" contain ></v-img>
                <v-row>
                    <v-col class="justify-space-around pa-0">
                        <p class="text-center black--text title parag-1">{{dateDailyFormatter(weatherData.daily[i].dt)}}</p>
                        <p class="text-center grey--text caption parag-2">{{capitalize(weatherData.daily[i].weather[0].description)}}</p>
                    </v-col>
                </v-row>
                <v-spacer/>
                <p class="text-center blue--text pr-2 pt-5">{{weatherData.daily[i].humidity}}%</p>
            </div>
        </v-card>
    </v-main>
</template>

<script>
export default {
    name:'Daily',
    data:() =>{},
    props:['weatherData'],
    methods:{
        dateDailyFormatter (unixTime){
            let d = new Date(unixTime * 1000);

            let days = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
            let months = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"];

            let dayOfMonth = d.getDate();
            let day = days[d.getDay()];
            let month = months[d.getMonth()];

            return `${day},${month} ${dayOfMonth}`;
        },
        capitalize(weather){
            return weather.charAt(0).toUpperCase() + weather.slice(1);
        }
    }

}
</script>

<style>
.parag-1{
  margin-top: 23px;
}
.parag-2{
  margin-top:-20px;
}
</style>