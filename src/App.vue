<template>
  <div id="app">
    <main class="app-functional">
      <div class="search-box">
        <h1 class="title-app">Weather App</h1>
        <input
            type="text"
            placeholder="Search..."
            class="search-bar"
            v-model="city"
            @keypress="fetchWeather"
        />
        <h4 class="error-massage" v-if="weather.message === 'city not found'">
          please write the correct name of the city
        </h4>
        <div v-if="typeof weather.main != 'undefined'" class="weather-wrap">
          <div class="location">
            {{ weather.name }}
            <span class="country-weather">{{ weather.sys.country }}</span>
          </div>
          <div class="date">{{dateBuilder()}}</div>
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">
            {{ weather.weather[0].main }}
            <span class="weather-description">{{
                weather.weather[0].description
              }}</span>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      city: "",
      weather: {},
      url_base: "https://api.openweathermap.org/data/2.5/",
      api_key: "7d57ade6b0b3a45ab8bb59799344ed0a",
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key === "Enter") {
        fetch(
            `${this.url_base}weather?q=${this.city}&units=metric&APPID=${this.api_key}`
        )
            .then((res) => {
              return res.json();
            })
            .then(this.setResult);
      }
    },
    setResult(results) {
      this.weather = results;
    },
    dateBuilder () {
      let d = new Date();
      let months = ["January", "Feruary", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.search-box {
  margin-top: 40px;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  background: url("https://i.pinimg.com/564x/ff/00/03/ff00037d7981edf969be2ded9ef39f0b.jpg")
  no-repeat;
  background-size: cover;
  transition: 0.4s;

  overflow: hidden;
}
.app-functional {
  display: flex;
  justify-content: center;
  height: 100vh;
}

.search-bar {
  padding: 10px 15px;
  border-radius: 4px;
  border: none;
  box-shadow: 0 5px 10px 0 rgba(0, 0, 0, 0.25);
}
.search-bar::placeholder {
  color: #000;
  font-size: 18px;
}
.search-bar[type="text"] {
  font-size: 18px;
}
.search-bar:focus {
  outline: none;
}
.weather-wrap {
  margin-top: 40px;
  border-radius: 8px;
  box-shadow: 0 5px 10px 0 rgba(0, 0, 0, 0.5);
  padding: 30px;
  background-color: rgba(255, 255, 255, 0.35);
  font-size: 25px;
}
.weather-wrap .location {
  margin-bottom: 10px;
}
.weather-wrap .temp {
  margin-bottom: 10px;
}
.weather-wrap .weather {
  margin-bottom: 10px;
}
.location .country-weather {
  margin-bottom: 10px;
  font-size: 20px;
}
.weather .weather-description {
  margin-bottom: 10px;
  font-size: 20px;
}
.title-app {
  filter: drop-shadow( 1px 2px 0 rgba(0, 0, 0, 0.25));
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}
.error-massage {
  margin-top: 20px;
  display:flex;
  justify-content: center;
}
.date {
  font-weight: lighter;
  font-size: 20px;
  margin-bottom: 10px;
}
</style>
