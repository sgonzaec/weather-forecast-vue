<script>
import WeatherForecast from "./components/weather-forecast/WeatherForecast.vue";
import NoteAlert from "./components/Note/Note.vue";

export default {
  name: "App",
  components: {
    WeatherForecast,
    NoteAlert
  },
};
</script>

<template>
  <WeatherForecast />
  <NoteAlert />
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap");

body {
  box-sizing: border-box;
  font-family: "Roboto", sans-serif;
  margin: 0;
  padding: 0;
  font-family: "Roboto", sans-serif;
  background-color: #f5f5f5;
}
</style>
