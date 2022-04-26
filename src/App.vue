<template>
  <div id="app" :style="bgColor">
    <MDBContainer>
      <MDBRow class="search-bar">
        <MDBCol lg="6" offsetLg="3">
          <!-- <MDBInput
            label="Search.."
            type="text"
            v-model="searchTerm"
            @keyup.enter="getCoordinates()"
          /> -->

          <div class="search-box">
            <input
              type="text"
              class="search-bar"
              placeholder="Search..."
              v-model="searchTerm"
              @keyup.enter="getCoordinates()"
            />
          </div>

          <!-- <MDBBtn @click="getCoordinates()">Search</MDBBtn> -->
        </MDBCol>
      </MDBRow>
      <div class="weather-wrap" v-if="Object.keys(response).length !== 0">
        <MDBRow>
          <MDBCol lg="6" offsetLg="3">
            <div class="loc">
              <h2 class="text-center city">
                {{ response.name }}, {{ response.sys.country }}
              </h2>
              <h4 class="text-center">{{ timestamp }}</h4>
            </div>
            <div class="weather">
              <h1 class="text-center temp">
                {{ Math.round(response.main.temp) }}°
                <MDBIcon
                  :icon="getIcon(response.weather[0].main, response.clouds.all)"
                  size="sm"
                  class="icon"
                />
              </h1>
              <h3 class="text-center desc">{{ response.weather[0].main }}</h3>
            </div>
          </MDBCol>
        </MDBRow>

        <MDBRow>
          <WeatherForecast :forecast="forecast" />
        </MDBRow>
      </div>
    </MDBContainer>
  </div>
</template>



<script>
import WeatherForecast from "./components/WeatherForecast.vue";
// import { MDBInput } from "mdb-vue-ui-kit";
import { MDBCol, MDBRow, MDBContainer, MDBIcon } from "mdb-vue-ui-kit";

export default {
  components: {
    MDBCol,
    MDBRow,
    MDBContainer,
    WeatherForecast,
    MDBIcon,
  },
  data() {
    return {
      apiKey: "3a23e9e6b5792164c1e419bd83bb7c08",
      searchTerm: "",
      lat: 51.5073,
      lon: -0.1276,
      forecast: {},
      response: {},
      timestamp: "",
    };
  },
  created() {
    setInterval(() => {
      this.getHour();
    }, 1000);

    fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=London&appid=${this.apiKey}&units=metric`
    )
      .then((res) => {
        return res.json();
      })
      .then((json) => {
        this.response = json;
        console.log(json);
      });
    fetch(
      `https://api.openweathermap.org/data/2.5/onecall?lat=${this.lat}&lon=${this.lon}&exclude=minutely,hourly,alerts&appid=${this.apiKey}&units=metric`
    )
      .then((res) => {
        return res.json();
      })
      .then((json) => {
        this.forecast = json;
        console.log(json);
      });
  },
  methods: {
    getIcon(forecastData, cloudsPercentage) {
      if (forecastData === "Clear") {
        return "sun";
      } else if (forecastData === "Clouds") {
        if (cloudsPercentage > 40) {
          return "cloud";
        } else return "cloud-sun";
      } else if (forecastData === "Rain") {
        return "cloud-showers-heavy";
      } else if (forecastData === "Snow") {
        return "snowflake";
      }
    },

    getForecast() {
      fetch(
        `https://api.openweathermap.org/data/2.5/onecall?lat=${this.lat}&lon=${this.lon}&exclude=minutely,hourly,alerts&appid=${this.apiKey}&units=metric`
      )
        .then((res) => {
          return res.json();
        })
        .then((json) => {
          this.forecast = json;
          console.log(json);
        });
    },
    getWeather() {
      fetch(
        `https://api.openweathermap.org/data/2.5/weather?lat=${this.lat}&lon=${this.lon}&appid=${this.apiKey}&units=metric`
      )
        .then((res) => {
          return res.json();
        })
        .then((json) => {
          this.response = json;
          // console.log(this.response);
        });
    },
    getCoordinates() {
      fetch(
        `http://api.openweathermap.org/geo/1.0/direct?q=${this.searchTerm}&limit=5&appid=${this.apiKey}`
      )
        .then((res) => {
          return res.json();
        })
        .then((json) => {
          // console.log(json);
          this.lat = json[0].lat;
          this.lon = json[0].lon;
          this.getWeather();
          this.getForecast();
        });
    },
    getHour() {
{
      const today = new Date();
      const date = today.getFullYear() + '-' + (today.getMonth() + 1) + '-' + today.getDate();
      const time = today.getHours() + ":" + today.getMinutes() + ":" + today.getSeconds();
      const dateTime = date + ' ' + time;
      this.timestamp = dateTime;
    }
    },
  },
  computed: {
    bgColor() {
      let color = "";

      if (typeof this.response.main === "undefined") {
        color = "#F6BD60";
      } else if (this.response.main.temp > 15 && this.response.main.temp < 25) {
        color = "#F6BD60";
      } else if (this.response.main.temp < 15 && this.response.main.temp > 0) {
        color = "#D6F6DD";
      } else if (this.response.main.temp > 25) {
        color = "#E55934";
      } else {
        color = "#7C98B3";
      }
      return `background-color: ${color}`;
    },
  },
};
</script>

<style>
.icon {
  color: #fff;
}
#app {
  font-family: Roboto, Helvetica, Arial, sans-serif;
  /* background-color: rgb(194, 194, 237); */
  min-height: 100vh;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.15),
    rgba(0, 0, 0, 0.5)
  );
}

.search-box {
  width: 100%;
  margin: 30px 0 !important;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.desc,
.loc h2,
h4 {
  color: white;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.desc {
  font-size: 48px;
}

.city {
  padding: 24px 0;
}

.temp {
  background-color: rgba(255, 255, 255, 0.25);
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  color: white;
  border-radius: 16px;
  font-size: 76px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  margin: 24px 0;
  font-weight: 600;
}
</style>
