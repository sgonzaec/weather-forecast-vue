<script>
export default {
  props: {
    data: Object,
  },
};
</script>

<template>
  <div class="weather-forecast">
    <h3>Hi! the weather to {{ data?.name }} is:</h3>
    <span class="clouds">Clouds: {{ data?.clouds?.all }}%</span>
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
