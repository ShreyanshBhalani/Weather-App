<template>
  <div id="app"  :class="typeof weather.main != 'undefined' && weather.main.temp < 22 ? 'cold' : ''">
    
        
    <main>
      <div class="title">
        <h2>Weather App</h2>
      </div>
        <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Weather in Your City!"
          v-model="query"
          @keypress="fetchWeather"
          
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'app',

  data () {
    return {
      api_key: '34f1f7c58777541f2dff1205602965e9',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
     
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      let hours=d.getHours();
      let min=d.getMinutes();
      let sec=d.getSeconds();
      return `${day} ${date} ${month} ${year}      ${hours}: ${min}: ${sec}`;
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
  font-family: 'montserrat', sans-serif;
}
.title{
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 25px;
  font-style:italic;
  letter-spacing: 3px;
  overflow: hidden;
  animation: typing 4s;
  animation-iteration-count: infinite;
  white-space:nowrap;
  background: whitesmoke;
  margin-bottom: 7%;
}
@keyframes typing{
     0%{
         width:0ch;
     }
     
     75%{
         width:30ch;
     }
     100%{
         width:0ch;
     }
     
 }

main{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
#app {
  background-image: url('./assets/Clouds.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.cold {
  background-image: url('./assets/Cold1.jpg');
  background-size:cover;
}
main {
  min-height: 100vh;
  padding: 25px;
}
.search-box {
  width: 50%;
  margin-bottom: 30px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  
  color: #313131;
  font-size: 20px;
  appearance: none;
  border:none;
  outline: none;
  background: none;
  box-shadow: 0px 4px 8px black;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weather-box {
  margin-top:2%;
  text-align: center;
 
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather{
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

</style>