<script setup>
import WeatherInfo from "../WeatherInfo/WeatherInfo.vue";
</script>

<template>
  <nav>
    <label for="searchbar">TuClima.com</label>
    <input
      type="search"
      placeholder="Ingresa una ciudad"
      v-model="handleCityChange"
      @change="saveInput"
      @blur="saveInput"
    />
  </nav>
  <div class="weather-forecast">
    <WeatherInfo :city="savedValue" />
  </div>
  <footer>
    <p>Powered by <a href="#" title="simon g software">Simon G Software</a></p>
  </footer>
</template>

<script>
export default {
  data() {
    return {
      handleCityChange: "",
      savedValue: "",
    };
  },
  methods: {
    saveInput() {
      const API_KEY = "5a8678bb2ad91d757eb08826559747e6";
      const ubicacion = this.handleCityChange; // Reemplaza con la ciudad y país deseados

      const url = `https://api.weatherstack.com/current?access_key=${API_KEY}&query=${ubicacion}`;

      fetch(url)
        .then((response) => response.json())
        .then((data) => {
          console.log("Datos del clima:", data);
        })
        .catch((error) => {
          console.error("Error al obtener los datos del clima:", error);
        });

      this.savedValue = this.handleCityChange;
    },
  },
};
</script>

<style scoped>
nav {
  background-color: #1976d2;
  display: flex;
  min-height: 6em;
  justify-content: center;
  padding: 1em 0;
  gap: 1em;
  align-items: center;
  flex-direction: column;
}
footer {
  background-color: #1976d2;
  display: flex;
  min-height: 3em;
  justify-content: center;
  padding: 1em 0;
  color: white;
  position: fixed;
  width: 100%;
  bottom: 0;
  gap: 1em;
  align-items: center;
  flex-direction: column;
}
footer a {
  color: white;
  text-emphasis: none;
  text-decoration: none;
}
label {
  color: white;
  font-size: 1.5em;
}
input[type="search"] {
  width: 80%;
  height: 3em;
  border: transparent;
  border-radius: 30px;
  padding: 15px;
}
</style>
