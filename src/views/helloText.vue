<template>
  <div class="mainCard">
    <div class="textFrontCard">
      <vueFlashcards
        front="Click Here for Weather forecast"
        :back="weatherText"
        colorFront="#138496"
        colorBack=""
        colorTextFront="#DCDCDC"
        colorTextBack="#0074D9"
        textSizeFront="40px"
        textSizeBack="40px"
        headerFront=""
        headerBack=""
        footerFront=""
        footerBack=""
        v-if="shouldLoad"
      ></vueFlashcards>
    </div>
    <div v-if="shouldLoad" class="buttonDiv">
      <button class="big-button" @click="detailVisibility = !detailVisibility">Press me to see a detailed Data from API</button>
    </div>
    <div class="jsonData">
      <JSONView v-if="detailVisibility" :data="weatherData" class="jsonDataComp"></JSONView>
    </div>
  </div>
</template>

<script>
import vueFlashcards from 'vue-flashcard';
import axios from 'axios';
import { JSONView } from 'vue-json-component';

export default {
  components: {
    vueFlashcards,
    JSONView,
  },
  data() {
    return {
      detailVisibility: false,
      weatherData: '',
      shouldLoad: false,
      weatherText: '',
      errorText: '',
      temperatuere: '',
      apiKey: process.env.VUE_APP_APIKEY,
      coordinates: {
        lat: 0,
        lon: 0,
      },
    };
  },
  created() {
    this.getCordinates();
    this.getWeatherData(this.coordinates.lat, this.coordinates.lon, this.apiKey);
  },
  methods: {
    getCordinates() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition((getPosition) => {
          this.coordinates.lat = getPosition.coords.latitude;
          this.coordinates.lon = getPosition.coords.longitude;
          return getPosition;
        });
      } else {
        console.log('Error Reading Geolocation');
        // const warnText = 'Plz allow geolocation access';
      }
    },
    getWeatherData(lat, lon, apiKey) {
      axios
        .get(`https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${apiKey}&units=metric`)
        .then((data) => {
          this.weatherText = data.data.main.feels_like.toString().concat('° celcius');
          this.weatherData = data.data;
          this.shouldLoad = true;
        })
        .catch((err) => {
          this.errorText = 'Something Went Wrong';
          console.log(err);
        });
    },
  },
};
</script>

<style scoped>
.textFrontCard {
  min-width: 18%;
  max-width: 30%;
  margin: 0 auto;
}

.buttonDiv {
  margin: 50px;
}

.jsonData {
  max-width: 40%;
  text-align: left;
  margin: 0 auto;
}
.jsonDataComp{
    border: 1px solid grey;
}
</style>
