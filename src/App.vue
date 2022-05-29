<template>
<div v-bind:class="{hot, cold, cloud, rain}">
  <main>
    <div class="search">
      <input type='text' class="search-bar" placeholder="Search....."
      v-model="query" 
      @keypress="flethWeather"
      />
    </div>

    <div class="weather" v-if="typeof weather.main != 'undefined'">
      <div class="location-date">
        <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
        <div class="date">{{dateBuilder ()}}</div>
      </div>

      <div class="weather-temp">
        <div class="temp">{{Math.round(weather.main.temp)}}°C</div>
        <div class="weather">{{ weather.weather[0].main }}</div>
        <div class="temp-feel">Real Feeling: {{Math.round(weather.main.feels_like)}}°C</div>
        <div class="temp-feel">Wind: {{Math.round(weather.wind.speed)}}M/S</div>
      </div>
    </div>
  </main>

</div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return {
      api_key: 'beaf898fbe6fa40d0536b34df42a67ef',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      hot: true,
      cold: true,
      cloud: true,
      rain: true
    }
  },
methods: {
  flethWeather(e){
    if(e.key === "Enter"){
      console.log(this.weather)
      fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
      .then(res => {
        return res.json();
      }).then(this.setResults)
      .then(this.changeUrl);
    }
  },
  setResults (result){
    this.weather = result;
  },
   changeUrl(){
    if(this.weather.main.temp >= 28){
      this.hot = true;
      this.cold = false
    }else if(this.weather.main.temp < 30){
      this.hot = false;
      this.cold = true
      console.log(this.weather.weather[0].main)
    }
    if(this.weather.weather[0].main === 'Clouds'){
      this.cloud = true;
       this.rain = false
    }else if(this.weather.weather[0].main === 'Rain'){
      this.rain = true;
      this.cloud = false
    }else{
      this.cloud = false;
      this.rain = false
    }
  },
   dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
}


}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family:'Times New Roman', Times, serif
}
.hot {
  background-image: url(./assets/sun.png);
  background-size: cover;
  background-position: bottom;
  transition: 1s;
 
}
.cold {
  background-image: url(./assets/cold.png);
  background-size: cover;
  background-position: bottom;
  transition: 1s;
}
.cloud {
  background-image: url(./assets/Cloud.png);
  background-size: cover;
  background-position: bottom;
  transition: 1s;
}
.rain {
  background-image: url(./assets/rain.png);
  background-size: cover;
  background-position: bottom;
  transition: 1s;
}

main{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(rgba(0, 0, 0, 0.1), rgba(0, 0, 0, 0.1));
}
.search {
  width: 100%;
  margin-bottom: 30px;
}

.search .search-bar{
  display: block;
  width: 100%;

  padding: 15px;
  color: rgb(229, 255, 255);
  font-size: 30px;
  border: none;
  background: none;
  appearance: none;
  background-color: rgba(150, 205, 205, 0.9);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s
  }
  .search .search-bar:focus{
    background-color: rgba(150, 205, 205, 0.8);
    border-radius: 16px 0px 16px 0px;
  }
  .location-date .location{
    color: white;
    font-size: 30px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }
  .location-date .date{
    color: white;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }
  .weather-temp{
    text-align: center;
  }
  .weather-temp .temp{
    display: inline-block;
    padding: 10px 25px;
    color: white;
    font-size: 100px;
    font-weight: 900;

    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba(2555, 255, 255, 0.25);
    border-radius: 35px;
    margin: 30px 0px;
  }

  .weather-temp .weather{
    color: white;
    font-size: 50px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }
  .temp-feel{
    padding: 10px 25px;
    color: white;
    font-size: 30px;
    font-weight: 50;

    background-color: rgba(255, 255, 255, 0.25);
    border-radius: 35px;
    margin: 30px 0px;
    
  }
</style>
