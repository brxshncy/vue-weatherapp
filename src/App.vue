<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm': ''">
    <main>
      <div class="search-box">
        <input type="text" name="" id="" placeholder="Enter Area here" v-model="query" v-on:keypress="fetchWeather"  class="search-bar">
      </div>
      <div class="weather-wrap" v-if="typeof weather.main !='undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }} {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
      </div>
      <div class="weather-box" v-if="typeof weather.main !='undefined'">
        <div class="temp">{{ parseInt(weather.main.temp) }} °C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
      </div>
    </main>
  </div>
</template>

<script>


export default {
  name:'app',
  data(){
    return{
        api_key: '1bdb35425c82cb76ec9895c1fd73ee9f',
        url_base:'api.openweathermap.org/data/2.5/',
        query: '',
        weather:{},
    }
  },
  methods:{
    fetchWeather(e){
      if(e.key =="Enter"){
        fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.query}&units=metric&appid=${this.api_key}`)
        .then(res => {
           return res.json();
         }).then(this.setResults);
      }
    },
    setResults(results){
      console.log(results);
      this.weather = results;
    },
    dateBuilder(){
      let d = new Date();
      let months = ["January","February","March","April","May","June","July","August","September","October","November","December"];
      let days = ["Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month}, ${year}`;
    }
  }
}
</script>

<style>
*{
  margin:0;
  padding:0;
  box-sizing:border-box;
}
body{
  font-family:'montserrat', sans-serif;
}
#app{
  background:url('./assets/cold-bg.jpg') no-repeat center center/cover;
  transition: 0.4s;
}
#app.warm{
  background:url('./assets/warm-bg.jpg') no-repeat center center/cover;
}
main{
  min-height:100vh;
  padding:25px;
  background-image:linear-gradient(to bottom,rgba(0,0,0,0.25),rgba(0,0,0,0.75));
}
.search-box{
  width:100%;
  margin-bottom:30px;
}
.search-box .search-bar{
  display:block;
  width:100%;
  padding:15px;
  color:#313131;
  font-size:20px;
  appearance:none;
  border:none;
  outline:none;
  background: none;
  box-shadow:0px 0px 8px rgba(0, 0,0,0.25);
  background-color:rgba(255,255,255,0.5);
  border-radius:0px 16px 0px 16px;
  transition: 0.4s;

}
.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
  background-color:rgba(255,255,255,0.75);
  border-radius:16px 0px 16px 0px;
}
.location-box .location{
  color:white;
  font-size:32px;
  font-weight:500;
  text-align:center;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}
.location-box .date{
  color:#fff;
  font-size:20px;
  font-weight:100;
  font-style:italic;
  text-align:center
}
.weather-box{
  text-align:center;
}
.weather-box .temp{
  display:inline-block;
  padding:10px 25px;
  color:#fff;
  font-size:102px;
  font-weight:900;
  text-shadow:3px 6px rgba(0,0,0,0.25);
  background-color:rgba(255,255,255,0.25);
  border-radius:16px;
  margin:30px 0px;
  box-shadow:3px 6px rgba(0,0,0,0.25);
}
.weather-box .weather{
  color:#fff;
  font-size:48px;
  font-weight: 700;
  font-style:italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}
</style>
