<template>
  <div id="app" :class="typeof wheather.main != 'undefined' && wheather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input 
        type="text" 
        name="" 
        id="" 
        class="search-bar" 
        placeholder="Busca..."
        v-model="query"
        @keypress="fetchWeather"
        />
      </div>

      <div class="wheather-wrap" v-if="typeof wheather.main != 'undefined'">

        <div class="location-box">
          <div class="location">{{wheather.name}}, {{wheather.sys.country}}</div>
          <div class="date">{{dateBuilder()}}</div>
        </div>

        <div class="wheather-box">
          <div class="temp">{{ Math.round(wheather.main.temp) }}°</div>
          <div class="wheather">{{wheather.weather[0].main}}</div>
        </div>

      </div>
      
    </main>
  </div>
</template>

<script>



export default {
  name: 'App',
  data (){
    return {
       api_key : '6e8f6f4211176a08ae72c29800353af3',
       url_base: 'https://api.openweathermap.org/data/2.5/',
       query: '',
       wheather: {}
    }
  },
  methods:{
    fetchWeather (e){
      if (e.key == "Enter"){
          fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }

    },
    setResults (results){
      this.wheather = results;
    },
    dateBuilder (){
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
    margin:0;
    padding:0;
    box-sizing: border-box;
  }
  
  body {
    font-family: 'Roboto'
  }
  
  #app { 
    background-image: url('./assets/cold-bg.jpg');
    background-size:cover;
    background-position:bottom;
    transition:0.4s;
  }

  #app.warm {
    background-image:url('./assets/warm-bg.jpg');
  }
  
  main {
    min-height: 100vh;
    padding:25px;

    background: rgb(255,255,255);
    background: linear-gradient(180deg, rgba(255,255,255,0.25) 0%, rgba(0,0,0,0.75) 110%);
    
  }
  
  .search-box{
    width:100%;
    margin-bottom:30px;
  }
  
  .search-box .search-bar {
    display:block;
    width:100%;
    padding:15px;
  
    color:#313131;
    font-size:20px;
  
    appearance:none;
    border:none;
    outline:none;
    background:none;
  
    
    box-shadow: 0px 0px 8px rgba(0,0,0,1);
    background-color:rgba(255,255,255,0.5);
    border-radius:0px 16px 0px 16px;
    transition:0.4s;
  
  }
  
  .search-box .search-bar:focus {
    box-shadow: 0px 0px 16px rgba(0,0,0,1);
    background-color: rgba(255, 255, 255, 0.75);
    border-radius:16px 0px 16px 0px;
  }

  .location-box .location {
    color:#ffff;
    font-weight:500;
    font-size:32px;
    text-align: center;
    text-shadow:1px 3px rgba(0,0,0,0.50);
  }

  .location-box .date {
    color:#ffffff;
    font-size:20px;
    font-weight:300;
    font-style:italic;
    text-align:center;
    text-shadow:1px 2px rgba(0,0,0,0.50);

  }

  .wheather-box {
    text-align:center;
  }

  .wheather-box .temp {
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

  .wheather-box .wheather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
