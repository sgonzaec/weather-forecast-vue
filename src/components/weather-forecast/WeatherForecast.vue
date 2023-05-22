<script setup>
import WeatherInfo from "../WeatherInfo/WeatherInfo.vue";
</script>

<template>
  <nav>
    <label for="searchbar">TuClima.com</label>
    <div class="groupDataUser">
      <input
        type="search"
        placeholder="Ingresa una ciudad"
        v-model="handleCityChange"
        @change="manualLocation"
        @blur="manualLocation"
      />
      <button
        class="yourLocation"
        title="usa tu ubicación"
        @click="autoLocation"
      >
        &#x1f9ed;
      </button>
    </div>
  </nav>
  <div :class="{'mainContainer_loading': isLoading, 'mainContainer': !isLoading}">
    <div v-if="isLoading" class="lds-facebook">
      <div></div>
      <div></div>
      <div></div>
    </div>
    <div v-else style="display: contents;">
      <WeatherInfo v-if="savedValue !== ''" :data="savedValue" />
    </div>
  </div>
  <footer>
    <p>Powered by <a href="#" title="simon g software">Simon G Software</a></p>
  </footer>
</template>

<script>
export default {
  data() {
    return {
      isLoading: false,
      API_KEY: "a79bcd1f286c5df8c8cda3ec06202e20",
      handleCityChange: "",
      savedValue: "",
      isCurrentLocation: false,
    };
  },
  methods: {
    manualLocation() {
      //   const ubicacion = this.handleCityChange; // Reemplaza con la ciudad y país deseados

      const url = `http://api.openweathermap.org/data/2.5/weather?q=London,uk&APPID=${this.API_KEY}`;

      fetch(url, {
        method: "GET",
        headers: {
          "Content-Type": "application/json",
        },
      })
        .then((response) => response.json())
        .then((data) => {
          console.log("Datos del clima:", data);
        })
        .catch((error) => {
          console.error("Error al obtener los datos del clima:", error);
        });

      this.savedValue = this.handleCityChange;
    },
    autoLocation() {
      if (navigator.geolocation) {
        this.isLoading = true;
        navigator.geolocation.getCurrentPosition((position) => {
          this.isCurrentLocation = true;

          fetch(
            `https://api.openweathermap.org/data/2.5/weather?lat=${position.coords.latitude}&lon=${position.coords.longitude}&appid=${this.API_KEY}`,
            {
              referrer: "http://localhost:8080/",
              referrerPolicy: "strict-origin-when-cross-origin",
              body: null,
              method: "GET",
              mode: "cors",
              credentials: "omit",
              headers: {
                accept: "*/*",
                "accept-language": "en-US,en;q=0.9,es;q=0.8",
                "sec-ch-ua":
                  '"Google Chrome";v="111", "Not(A:Brand";v="8", "Chromium";v="111"',
                "sec-ch-ua-mobile": "?0",
                "sec-ch-ua-platform": '"macOS"',
                "sec-fetch-dest": "empty",
                "sec-fetch-mode": "no-cors",
                "sec-fetch-site": "cross-site",
              },
            }
          )
            .then((response) => response.json())
            .then((data) => {
              this.savedValue = data;
              this.isLoading = false;
            })
            .catch((error) => {
              console.error("Error al obtener los datos del clima:", error);
              this.isLoading = false;
            });
        });
      } else {
        alert("Geolocation is not supported by this browser.");
        this.isLoading = false;
      }
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
.mainContainer_loading {
  display: grid;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.mainContainer {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.groupDataUser {
  display: flex;
  width: 90%;
  gap: 1em;
  align-items: center;
  flex-direction: row;
  justify-content: space-between;
}
.yourLocation {
  background-color: transparent;
  border: none;
  font-size: 1.5em;
  color: white;
  cursor: pointer;
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
  width: 100%;
  height: 3em;
  border: transparent;
  border-radius: 30px;
  padding: 15px;
}
.lds-facebook {
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
  top: calc(50% - 2em);
  right: calc(50% - 2em);
}
.lds-facebook div {
  display: inline-block;
  position: absolute;
  left: 8px;
  width: 16px;
  background: #1976d2;
  animation: lds-facebook 1.2s cubic-bezier(0, 0.5, 0.5, 1) infinite;
}
.lds-facebook div:nth-child(1) {
  left: 8px;
  animation-delay: -0.24s;
}
.lds-facebook div:nth-child(2) {
  left: 32px;
  animation-delay: -0.12s;
}
.lds-facebook div:nth-child(3) {
  left: 56px;
  animation-delay: 0;
}
@keyframes lds-facebook {
  0% {
    top: 8px;
    height: 64px;
  }
  50%,
  100% {
    top: 24px;
    height: 32px;
  }
}
</style>
