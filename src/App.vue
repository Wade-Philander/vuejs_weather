<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : '' ">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search..." v-model="search_query" @keypress="fetchWeather"/>
        <!-- {{ search_query }}  -->
      </div>
      <div class="weather-wrap" v-if="typeof (weather.main)  != 'undefined' ">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather">
          <div class="temp">{{ Math.round (weather.main.temp) }}°c</div>
          <div class="current-weather">{{ weather.weather[0].main}} </div>
        </div>
      </div>
    </main> 
    
  </div>
</template>

<script>


export default {
  name: 'App',
  data(){
    return{

      api_key : '0823ea71a9f05d8a204622b106dd4c38',
      url: "api.openweathermap.org/data/2.5/",
      search_query:'',
      weather: {}
    }
  },
  methods:{
    fetchWeather(e){
      if (e.key == "Enter") {
        console.log('pressed enter')
        fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.search_query}&units=metric&appid=0823ea71a9f05d8a204622b106dd4c38`)
          .then(res => {
            this.weather = res.json(),
            console.log(this.weather)
            return this.weather;
          }).then(this.setResults); 
      }
    },
    setResults(results){
     this.weather = results; 
    },
    dateBuilder() {

        //const current = new Date();
        //const date = `${current.getDate()}/${current.getMonth()+1}/${current.getFullYear()}`;      
        //return date;

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
    font-family: 'montserrat', sans-serif;
  }
  #app{
    background-image:url('./assets/cold-bg.jpg');
    background-size: cover;
    background-position: bottom;
    transition: 0.4s;
  }
  #app.warm{
    background-image: url('./assets/warm-bg.jpg');
  }

  main{
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0,0,0, 0.25), rgba(0, 0,0, 0.75));
  }

  .search-box {
    width: 100%;
    margin-bottom: 30px;
  }
  .search-box .search-bar{
    display:block;
    width: 100%;
    padding: 15px;
    color: #313131;
    font-size: 20px;
    appearance: none;
    border: none;
    outline: none;
    background: none;
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 0px 16px 0px 16px;
    transition:0.4s
  }

  .search-box .search-bar:focus{
    box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
    background-color:rgba(255, 255, 255, 0.75);
    border-radius: 16px 0px 16px 0px;
    
  }
  .location-box .location{
    color: whitesmoke;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }

  .location-box .date{
    color: whitesmoke;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }
  .weather{
    text-align: center;
  }

  .weather .temp{
    display: inline-block;
    padding: 10px 25px;
    color: white;
    font-size: 102px;
    font-weight: 900;
    text-shadow: 3px 6px rgba(0,0,0,0.25);
    background-color: rgba(255, 255, 255, 0.25);
    border-radius:16px;
    margin: 30px 0px;
    box-shadow:3px 6px rgba(0,0,0,0.25);
  }

  .weather .current-weather {
    color: white;
    font-size: 48px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba(0,0,0,0.25);
  }

</style>
