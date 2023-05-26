<script>
export default {
  props: {
    data: Object,
  },
  methods: {
    convertCelsius(Temp) {
      return Math.round(Temp - 273.15);
    }
  }
};
</script>

<template>
  <div class="weather-forecast">
    <h3>Hi! the weather to {{ data?.name }} is:</h3>
    <div class="dataInfo">
      <span class="item">Humidity: {{ data?.main?.humidity }}%</span>
    <span class="item">Clouds: {{ data?.clouds?.all }}%</span>
      <span class="item">Temperature: {{ convertCelsius(data?.main?.temp) }}° C</span>
      <span class="item">Feels Like: {{ convertCelsius(data?.main?.feels_like) }}° C</span>
    </div>
    <div class="weather">
      <img
        :src="`https://openweathermap.org/img/wn/${data?.weather?.[0]?.icon}@2x.png`"
        alt="icon_weather"
        class="icon"
      />
      <p class="description">{{ data?.weather?.[0]?.main }}</p>
    </div>
  </div>
</template>

<style scoped>
.dataInfo{
  display: grid;
  grid-template-columns: 1fr 1fr;
}
.item{
  margin: 0.5em;
}
.weather-forecast {
  width: 90%;
  margin: auto;
  background-color: white;
  display: flex;
  margin-top: 2em;
  border-radius: 15px;
  flex-direction: column;
  align-items: center;
  box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.25);
}
.weather {
  display: flex;
  align-items: center;
}
</style>
