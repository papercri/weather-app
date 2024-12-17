<template>
  <div class="container flex max-w-[780px] p-10 gap-y-10">
    <header class="head w-full mt-0 m-x-auto flex justify-between ">
      <h1 class="text-3xl text-left">MyWeatherApp</h1>
      <ul class="flex pt-2 gap-x-2 hidden">
        <li class="font-bold border-b border-fuchsia-700 px-8">Today</li>
        <li class="">Next 4 days</li>
      </ul>
    </header>
  <!-- form -->
    <main clas="main w-full mt-0 m-auto  flex justify-center">
      <div class="flex ml-[30px]">
        <form class="w-full">
          <input
              class="getWeat text-black"
              type="text"
              placeholder="Search location"
              v-model="location"
            />
            <button class="butt" @click.prevent="getWeather">Get Weather</button>
        </form>
      </div>
 <!-- show data -->
      <div v-if="weather" class="flex flex-col todayBox text-base">
        <div class="flex w-full justify-between">
          <span class="city font-bold">{{ this.city }}, {{ this.country }}</span>
          <span class="date">{{ weekday }} {{currentDate()}}</span>
        </div>
        <div class="flex w-full items-center py-10 flex-col">
          <div class="temp">
            <span class="text-4xl">{{ this.temp }} °C</span>
            <img :src="`https://openweathermap.org/img/wn/${this.icon}@2x.png`" width="100"/>
          </div>
          <p class="capitalize text-sm font-bold">{{ this.description }}</p>
        </div>
        <div class="flex gap-4 justify-between">
          <div class="flex flex-col items-center text-xs uppercase">
            <span>Temp Min.</span>
            <span>{{ this.tempMin }}  °C</span>
          </div>
          <div class="flex flex-col items-center text-xs uppercase">
            <span>Temp Max.</span>
            <span>{{ this.tempMax }} °C</span>
          </div>
          <div class="flex flex-col items-center text-xs uppercase">
            <span>Feels</span>
            <span>{{ this.feels }} °C</span>
          </div>
          <div class="flex flex-col items-center text-xs uppercase">
            <span>Humidity</span>
            <span>{{ this.humidity }} %</span>
          </div>
          <div class="flex flex-col items-center text-xs uppercase">
            <span>Wind</span>
            <span>{{ this.windSpeed }} m/s</span>
          </div>
          
        </div>
    
      
      </div>
      <div v-if="forecast" class="flex nentHoursBox text-xs gap justify-between mt-5">
        <div class="forecast-box relative">
          <div class="fhour text-center">12:00 AM</div>
          <div class="fimage  relative -top-4"><img :src="`https://openweathermap.org/img/wn/${this.forecast.list[0].weather[0].icon}@2x.png`" width="100"/></div>
          <div class="ftemp  relative -top-8 text-center text-2xl">{{Math.round(this.forecast.list[0].main.temp)}} °C</div>
        </div>
        <div class="forecast-box relative">
          <div class="fhour text-center">15:00 PM</div>
          <div class="fimage  relative -top-4"><img :src="`https://openweathermap.org/img/wn/${this.forecast.list[1].weather[0].icon}@2x.png`" width="100"/></div>
          <div class="ftemp  relative -top-8 text-center text-2xl">{{Math.round(this.forecast.list[1].main.temp)}} °C</div>
        </div>
        <div class="forecast-box relative">
          <div class="fhour text-center">18:00 AM</div>
          <div class="fimage  relative -top-4"><img :src="`https://openweathermap.org/img/wn/${this.forecast.list[2].weather[0].icon}@2x.png`" width="100"/></div>
          <div class="ftemp  relative -top-8 text-center text-2xl">{{Math.round(this.forecast.list[2].main.temp)}} °C</div>
        </div>
        <div class="forecast-box relative">
          <div class="fhour text-center">21:00 PM</div>
          <div class="fimage  relative -top-4"><img :src="`https://openweathermap.org/img/wn/${this.forecast.list[3].weather[0].icon}@2x.png`" width="100"/></div>
          <div class="ftemp  relative -top-8 text-center text-2xl">{{Math.round(this.forecast.list[3].main.temp)}} °C</div>
        </div>

      </div>
      <div v-if="loading" class="loader">Loading....</div>
      <div v-if="error">{{error}}</div>
    </main>
 
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      city:null,
      country:null,
      temp:null,
      tempMax:null,
      tempMin:null,
      feels:null,
      humidity:null,
      windSpeed:null,
      description:null,
      icon:null,
      location: "",
      weather: null,
      forecast: null,
      error: null,
      loading: false,
    };
  },
  methods: {
    currentDate() {
      const current = new Date();
      const date = `${current.getDate()}/${current.getMonth()+1}/${current.getFullYear()}`;
      return date;
    },
    getForecast() {
      this.forecast = null;
      fetch( `https://api.openweathermap.org/data/2.5/forecast?q=${this.location}&cnt=4&appid=${import.meta.env.VITE_API_KEY}&units=metric&lang=en`)
      .then(response => {
        return response.json();      
      })
      .then(data => {
        console.log(data);
        this.forecast = data;     
      })
      .catch(error => console.log(error))
    },
    getWeather() {
      this.loading = true;
      this.weather = null;
      this.error = "";
      fetch( `https://api.openweathermap.org/data/2.5/weather?q=${this.location}&appid=${import.meta.env.VITE_API_KEY}&units=metric&lang=en`)
     
      .then((resultado)=>{
        //console.log(resultado)
        if(!resultado.ok) throw new Error("Problema en buscar los datos")
        return resultado.json(); 
      })
      .then(data => {
        //console.log(data);
        this.weather = data;
        this.city = this.weather.name ? this.weather.name : undefined;
        this.country = this.weather.sys.country ? this.weather.sys.country : undefined;
        this.temp = Math.round(this.weather.main.temp);
        this.tempMax = Math.round(this.weather.main.temp_max)
        this.tempMin = Math.round(this.weather.main.temp_min);
        this.humidity = this.weather.main.humidity ? this.weather.main.humidity : undefined;
        this.windSpeed = this.weather.wind.speed ? this.weather.wind.speed  : undefined;
        this.feels = Math.round(this.weather.main.feels_like);
        this.description = this.weather.weather[0].description ? this.weather.weather[0].description : undefined;
        this.icon = this.weather.weather[0].icon ? this.weather.weather[0].icon : undefined;

        this.getForecast();
       console.log(data);
      })
      .catch((error)=>{
        console.log(error.message)
        this.error = error.message
      })
      .finally(()=>{
        this.location = ""
        this.loading = false;
      })
    },
    
  },
  computed: {
    weekday: function () {
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday" ];
      let today = new Date();
      let weekday = today.getDay();
      return days[weekday];
    }
  },
};
</script>

<style>
.container{
  background-color: rgba(255,255,255,0.05);
  width:100%;
 
  height:auto;
  margin:20px auto;
  justify-content: center;
  align-items: flex-start;
  align-content: flex-start;
  color:white;
  flex-wrap:wrap;
  padding:30px;
  font-family: "Parkinsans", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  border-radius: 30px;
}
.head{
  background-image: url("./img/cloud.png");
  background-position: top left;
  background-repeat: no-repeat;
  padding-left: 40px;
  padding-top: 25px;
  background-size: 50px;
}
.getWeat{
  position: relative;
  background-color: #D9D9D9;
  border: 1px solid #373535;
  border-radius: 20px;
  color:black;
  padding: 3px 3px 3px 40px;
  width:300px;
  font-weight:200;
  background-image: url("./img/search.png");
  background-position: 10px;
  background-repeat: no-repeat;
  background-size: 20px;
  outline: none;
}
.butt{
  background-color: #AD36CB;
  border: 1px solid #373535;
  border-radius: 20px;
  color:#ffffff;
  padding: 3px 10px ;
  position: relative;
  font-weight:600;
  left:-30px
 
}
.todayBox{
  background: rgb(173,54,203);
  background: linear-gradient(135deg, rgba(173,54,203,1) 31%, rgba(167,54,196,1) 51%, rgba(51,51,51,1) 86%);
  border-radius: 20px;
  padding: 25px;
  margin-top: 20px;
}
.forecast-box{
  width:20%;
  background: rgb(173,54,203);
  background: linear-gradient(180deg, rgba(173,54,203,1) 21%, rgba(167,54,196,1) 31%, rgba(173,54,203,0) 76%);
  border-radius: 20px;
  padding: 10px 5px;
  display:flex;
  flex-direction: column;
  justify-content: center;
  
}
.city{
  background-image: url("./img/place.png");
  background-position: right center;
  background-repeat: no-repeat;
  background-size: 12px;
  padding: 0 16px 0 0;
}
.temp{
  background-image: url("./img/icon-temp.png");
  background-position: left center;
  background-repeat: no-repeat;
  display: flex;
  align-items: center;
  padding: 0 0 0 40px;
}
.loader {
  color: rgb(173,54,203);
  font-size: 90px;
  text-indent: -9999em;
  overflow: hidden;
  width: 1em;
  height: 1em;
  border-radius: 50%;
  margin: 72px auto;
  position: relative;
  -webkit-transform: translateZ(0);
  -ms-transform: translateZ(0);
  transform: translateZ(0);
  -webkit-animation: load6 1.7s infinite ease, round 1.7s infinite ease;
  animation: load6 1.7s infinite ease, round 1.7s infinite ease;
}
@-webkit-keyframes load6 {
  0% {
    box-shadow: 0 -0.83em 0 -0.4em, 0 -0.83em 0 -0.42em, 0 -0.83em 0 -0.44em, 0 -0.83em 0 -0.46em, 0 -0.83em 0 -0.477em;
  }
  5%,
  95% {
    box-shadow: 0 -0.83em 0 -0.4em, 0 -0.83em 0 -0.42em, 0 -0.83em 0 -0.44em, 0 -0.83em 0 -0.46em, 0 -0.83em 0 -0.477em;
  }
  10%,
  59% {
    box-shadow: 0 -0.83em 0 -0.4em, -0.087em -0.825em 0 -0.42em, -0.173em -0.812em 0 -0.44em, -0.256em -0.789em 0 -0.46em, -0.297em -0.775em 0 -0.477em;
  }
  20% {
    box-shadow: 0 -0.83em 0 -0.4em, -0.338em -0.758em 0 -0.42em, -0.555em -0.617em 0 -0.44em, -0.671em -0.488em 0 -0.46em, -0.749em -0.34em 0 -0.477em;
  }
  38% {
    box-shadow: 0 -0.83em 0 -0.4em, -0.377em -0.74em 0 -0.42em, -0.645em -0.522em 0 -0.44em, -0.775em -0.297em 0 -0.46em, -0.82em -0.09em 0 -0.477em;
  }
  100% {
    box-shadow: 0 -0.83em 0 -0.4em, 0 -0.83em 0 -0.42em, 0 -0.83em 0 -0.44em, 0 -0.83em 0 -0.46em, 0 -0.83em 0 -0.477em;
  }
}
@keyframes load6 {
  0% {
    box-shadow: 0 -0.83em 0 -0.4em, 0 -0.83em 0 -0.42em, 0 -0.83em 0 -0.44em, 0 -0.83em 0 -0.46em, 0 -0.83em 0 -0.477em;
  }
  5%,
  95% {
    box-shadow: 0 -0.83em 0 -0.4em, 0 -0.83em 0 -0.42em, 0 -0.83em 0 -0.44em, 0 -0.83em 0 -0.46em, 0 -0.83em 0 -0.477em;
  }
  10%,
  59% {
    box-shadow: 0 -0.83em 0 -0.4em, -0.087em -0.825em 0 -0.42em, -0.173em -0.812em 0 -0.44em, -0.256em -0.789em 0 -0.46em, -0.297em -0.775em 0 -0.477em;
  }
  20% {
    box-shadow: 0 -0.83em 0 -0.4em, -0.338em -0.758em 0 -0.42em, -0.555em -0.617em 0 -0.44em, -0.671em -0.488em 0 -0.46em, -0.749em -0.34em 0 -0.477em;
  }
  38% {
    box-shadow: 0 -0.83em 0 -0.4em, -0.377em -0.74em 0 -0.42em, -0.645em -0.522em 0 -0.44em, -0.775em -0.297em 0 -0.46em, -0.82em -0.09em 0 -0.477em;
  }
  100% {
    box-shadow: 0 -0.83em 0 -0.4em, 0 -0.83em 0 -0.42em, 0 -0.83em 0 -0.44em, 0 -0.83em 0 -0.46em, 0 -0.83em 0 -0.477em;
  }
}
@-webkit-keyframes round {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}
@keyframes round {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}
</style>
