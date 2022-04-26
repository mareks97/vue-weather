
<template>
  <MDBRow :cols="['1', 'md-4']" class="g-3">
    <MDBCol v-for="day in forecast.daily" :key="day">
      <MDBCard class="h-100 card">
        <MDBCardBody>
          <MDBCardTitle class="title">{{
            getForecastDate(day.dt)
          }}</MDBCardTitle>
          <MDBCardText>
            <h3 class="desc">{{ day.weather[0].main }}</h3>
            <MDBRow>
              <MDBCol col="6">
                <h4>{{ Math.floor(day.temp.max) }}°</h4>
                <h4>{{ Math.floor(day.temp.min) }}°</h4>
              </MDBCol>
              <MDBCol col="6">
                <MDBIcon
                  :icon="getIcon(day.weather[0].main, day.clouds)"
                  size="3x"
                  class="icon"
                />
              </MDBCol>
            </MDBRow>
          </MDBCardText>
        </MDBCardBody>
      </MDBCard>
    </MDBCol>
  </MDBRow>
</template>

<script>
import {
  MDBCol,
  MDBRow,
  MDBCard,
  MDBCardBody,
  MDBCardTitle,
  MDBCardText,
  MDBIcon,
} from "mdb-vue-ui-kit";
export default {
  components: {
    MDBCol,
    MDBRow,
    MDBIcon,

    MDBCard,
    MDBCardBody,
    MDBCardTitle,
    MDBCardText,
  },
  data() {
    return {
      weekdays: [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ],
      months: [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ],
    };
  },
  props: {
    forecast: {},
  },
  methods: {
    getForecastDate(forecastInfo) {
      const d = new Date(forecastInfo * 1000);
      let dayNum = d.getDate();
      let dayWeek = this.weekdays[d.getDay()];
      let month = this.months[d.getMonth()];
      return `${dayWeek}, ${month} ${dayNum}`;
    },
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
  },
};
</script>

<style scoped>
.card {
  background-color: rgba(255, 255, 255, 0.25);
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.title {
  color: #fff;
  text-shadow: 2px 3px rgba(0, 0, 0, 0.25);
}

.card-body {
  padding: 18px !important;
}

.icon {
  color: #fff;
}
</style>