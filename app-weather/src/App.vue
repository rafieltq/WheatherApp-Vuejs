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

import '@/assets/Styles.css';

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

