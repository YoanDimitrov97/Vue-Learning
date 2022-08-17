<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search City..." v-model="search" @keypress="FetchWeather">
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined' ">
        <div class="location-box">
          <div class="location">
            {{weather.name}}, {{weather.sys.country}}
            <div class="date">
              {{ DateBuilder() }}
            </div>
          </div>
        </div>
        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}&#8451;</div>
          <div class="weather"> {{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import env from '@/env.js';
import {ref} from 'vue';
  export default {
    setup(){
      const search = ref("");
      const weather = ref({});
      const FetchWeather = (e) => {
        if(e.key == "Enter"){
          
          if(search.value != ""){
              fetch(`https://api.openweathermap.org/data/2.5/weather?q=${search.value}&units=metric&APPID=${env.apikey}`)
              .then(response => response.json())
              .then(data => {
                
                weather.value = data;
                search.value = "";
              })
          }
        }
      }

      const DateBuilder = () => {
        let d = new Date();
        let months = ['January', "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
        let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

        let day = days[d.getDay()];
        let date = d.getDate();
        let month = months[d.getMonth()];
        let year = d.getFullYear();

        return `${day} ${date} ${month} ${year}`;
      }

      return {
        search, FetchWeather, weather, DateBuilder
      }
    }
  }
</script>

<style lang="scss">
* {
  margin: 0;
  padding:0;
  box-sizing:border-box;

  body {
    margin:0 auto;
    //background-color:transparentize(#313131, 0.1);
    
    font-family:"montserrat", sans-serif;

    #app {
      transition:0.4s;
      background-image:url('./assets/mountain.jpg');
      background-size:cover;
      background-position:bottom; 
      width:100%;
      

      .warm {
        background-image:url('./assets/beach.jpg');
      }

      main {
        min-height:100vh;
        padding:25px;

        background-image:linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));

        .search-box {
          width:100%;
          
          margin-bottom:30px;
          display:flex;
          justify-content:center;

          .search-bar {
            display:block;
            width:100%;
            max-width:800px;
            padding:15px;
            color:#313131;
            font-size:20px;
            appearance:none;
            border:none;
            outline:none;
            background:none;
            box-shadow:0px 0px 8px rgba(0, 0, 0, 0.25);
            background-color:rgba(255, 255, 255, 0.5);
            border-radius:0px 16px 0px 16px;
            transition:0.4s;

            &:focus {
              background-color:rgba(255,255,255,0.75);
              box-shadow:0px 0px 16px rgba(0, 0, 0, 0.25);
              border-radius:16px 0px 16px 0px;
            }
          }
        }

        .location-box {
          .location {
            color:#fff;
            font-size:32px;
            font-weight:500;
            text-align:center;
            text-shadow: 1px 3px rgba(0, 0,0, 0.25);
          }

          .date {
            color:#fff;
            font-size:20px;
            font-weight:300;
            font-style:italic;
            text-align:center;
          }
        }

        .weather-box {
          text-align:center;

          .temp {
            display:inline-block;
            padding:10px 25px;
            color:#fff;
            font-size:102px;
            font-weight:900;

            text-shadow:3px 6px rgba(0, 0,0, 0.25);
            background-color:rgba(255,255,255, 0.25);
            border-radius:16px;
            margin:30px 0px;

            box-shadow:3px 6px rgba(0, 0, 0, 0.25);
          }

          .weather {
            color:#fff;
            font-size:48px;
            font-weight:700;
            font-style:italic;
            text-shadow:3px 6px rgba(0, 0,0, 0.25);
          }
        }

      }
    }
  }

}
</style>
