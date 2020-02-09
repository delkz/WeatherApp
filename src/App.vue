<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input 
        type="text" 
        class="search-bar" 
        placeholder="Busca..."
        v-model="query"
        @keypress="fetchWeather"
        >
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp" v-if="faren">{{ Math.round((weather.main.temp*1.8)+32) }}°F</div>
          <div class="temp" v-else>{{ Math.round(weather.main.temp) }}°C</div>
          <label class="switch">
            <input type="checkbox" v-model="faren">
            <span class="slider round"></span>
            <span>Fahrenheit</span>
          </label>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      api_key: '8b660efa87aae4469ac888f7725b36c5',
      url_base : 'http://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      faren: false
    }
  },
  methods:{
    fetchWeather(e){
      if(e.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res => {
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults(results){
      this.weather = results;
    },
    dateBuilder(){
      let d = new Date();
      let months = ['Janeiro','Fevereiro','Março','Abril','Maio','Junho','Julho','Agosto','Setembro','Outubro','Novembro','Dezembro'];
      let days = ['Domingo','Segunda','Terça','Quarta','Quinta','Sexta','Sabado'];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day}, ${date} ${month} ${year}`;
    }

  }

}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: 'montserrat', sans-serif;
}
#app{
  background-image: url('./assets/frio.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
main{
  min-height: 100vh;
  padding:25px;
  background-image: linear-gradient(to bottom,rgba(0,0,0,0.25),rgba(0,0,0,0.75))
}

.search-box{
  width: 100%;
  margin-bottom: 30px;
}
.search-bar{
  width: 100%;
  display: block;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: rgba(255,255,255,0.5);
  border-radius: 25px;
  transition: 0.4s;
}
.search-box .search-bar:focus{
  box-shadow: 0 0 16px rgba(0,0,0,0.25);
  background-color: rgb(255,255,255);
}
.location-box .location{
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.25)
}
.location-box .date{
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}
.weather-box{
  text-align: center;
}
.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color: white;
  font-size: 102px;
  font-weight: bold;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.25);
  border-radius: 25px;
  margin: 30px 0;
  box-shadow: 3px 6px rgba(0,0,0,0.25)

}
.weather-box .weather{
  font-size: 48px;
  font-weight: 700;
  color: #fff;
  font-style: italic;
  text-shadow: 3px 6px rgba(0,0,0,0.25)
}

#app.warm{
  background-image: url('./assets/calor.jpg');
}
.switch {
  position: relative;
  display: inline-block;
  width: 60px;
  height: 34px;
}

/* Hide default HTML checkbox */
.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

/* The slider */
.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: .4s;
  transition: .4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;
}

input:checked + .slider {
  background-color: #2196F3;
}

input:focus + .slider {
  box-shadow: 0 0 1px #2196F3;
}

input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}

/* Rounded sliders */
.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}
.switch > span:nth-child(3) {
 color: #fff;
 display: block;
 margin-left: 3.2em;
 font-size: 1.3em;
 margin-top: -.5em;
}
.switch {
 display: block;
 margin: 1em 25% 2em;
}

</style>
