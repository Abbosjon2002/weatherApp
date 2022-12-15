<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">

    <div class="main">

      <div class="search_box">
        <input type="text" class="search_bar" placeholder="Search..." v-model="query" @keypress.enter="fetchWeather">
      </div>


      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </div>
  </div>

</template>


<script>

export default {
  name: 'App',
  components: {},
  data() {
    return {
      api_key: '45d134a7442e365899bce0c74ca9f7f5',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}

    }
  },
  methods: {
    fetchWeather() {

      fetch(`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
          .then(res => {
            return res.json()
          }).then(this.setResults);
    },
    setResults(results) {
      this.weather = results
    },

    dateBuilder() {
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
  font-family: sans-serif;
}

#app {
  background-image: url("./assets/img/cold-bg.jpg");
  background-position: bottom;
  background-size: cover;
  transition: all 0.4s;
}

#app.warm {
  background-image: url("./assets/img/warm-bg.jpg");
}

.main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search_box {
  width: 100%;
  margin: 0 0 25px;
}

.search_box .search_bar {
  width: 100%;
  padding: 15px;
  display: block;
  color: #313131;
  font-size: 24px;
  appearance: none;
  background: none;
  outline: none;
  border: none;

  background-color: rgba(255, 255, 255, 0.5);
  transition: 0.4s;

}

.search_box .search_bar:focus {
  transition: 0.4s;
  background-color: rgba(255, 255, 255, 1);
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.27);


}

.location, .weather {
  text-align: center;
  font-weight: 600;
  font-size: 32px;
  margin-bottom: 10px;
  color: #fff;
  text-shadow: 2px 2px 5px #505050;
}

.date {
  text-shadow: 2px 2px 5px #505050;
  text-align: center;
  font-weight: 300;
  color: #fff;
  font-style: italic;
  font-size: 20px;
}

.weather-box {
  text-align: center;
}

.temp {
  display: inline-block;
  color: #fff;
  text-shadow: 2px 2px 5px #505050;
  box-shadow: 2px 2px 5px #505050;
  border-radius: 20px;
  font-size: 120px;
  font-weight: 900;
  padding: 10px 20px;
  background-color: #50505030;
  margin: 20px 0;
}

.weather {
  font-style: italic;
}
</style>
