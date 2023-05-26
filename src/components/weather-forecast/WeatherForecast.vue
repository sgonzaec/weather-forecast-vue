<script setup>
import WeatherInfo from "../WeatherInfo/WeatherInfo.vue";
</script>

<template>
  <nav>
    <label @click="savedValue = ''">WeatherForecast.com</label>
    <div class="groupDataUser">
      <input
        type="search"
        id="searchbar"
        placeholder="Ingresa una ciudad"
        v-model="handleCityChange"
        @keyup="getSuggestions"
      />
      <ul class="suggestionList" v-if="showSuggestions">
        <li
          v-for="suggestion in suggestions"
          :key="suggestion"
          @click="manualLocation(suggestion)"
        >
          {{ suggestion.description }}
        </li>
      </ul>
      <button
        class="yourLocation"
        title="usa tu ubicación"
        @click="autoLocation"
      >
        &#x1f9ed;
      </button>
    </div>
  </nav>
  <div :class="{ mainContainer_loading: isLoading, mainContainer: !isLoading }">
    <div v-if="isLoading" class="lds-facebook">
      <div></div>
      <div></div>
      <div></div>
    </div>
    <div v-else style="display: contents">
      <img
        v-if="savedValue === ''"
        src="../../assets/snowy-1.svg"
        alt="background"
        id="background_weather"
      />
      <p v-else-if="isFound === 0" class="errorText">
        We did not find matches, please try entering another city and verify
        that you do not have characters or symbols.
      </p>
      <WeatherInfo v-else :data="savedValue" />
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
      isValid: true,
      isFound: 1,
      savedValue: "",
      isCurrentLocation: false,
      suggestions: [],
      showSuggestions: false,
    };
  },
  methods: {
    validateText() {
      const regex = /^[a-zA-ZáéíóúÁÉÍÓÚñÑ\s]+$/;
      this.isValid = regex.test(this.savedValue);
    },
    quitarTildes(texto) {
      return texto.normalize("NFD").replace(/[\u0300-\u036f]/g, "");
    },
    manualLocation(suggestion) {
      this.validateText();
      if (this.handleCityChange === "" && !this.isValid) return;

      fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${this.quitarTildes(
          suggestion?.terms?.[0]?.value
        )}&appid=${this.API_KEY}`,
        {
          headers: {
            accept: "*/*",
            "accept-language": "en-US,en;q=0.9,es;q=0.8",
            "sec-ch-ua":
              '"Google Chrome";v="111", "Not(A:Brand";v="8", "Chromium";v="111"',
            "sec-ch-ua-mobile": "?0",
            "sec-fetch-dest": "empty",
            "sec-fetch-mode": "cors",
            "sec-fetch-site": "cross-site",
          },
          referrerPolicy: "strict-origin-when-cross-origin",
          body: null,
          method: "GET",
          mode: "cors",
          credentials: "omit",
        }
      )
        .then((response) => {
          if (response.status === 404) {
            this.isFound = 0;
            return;
          } else {
            this.isFound = 1;
          }
          return response.json();
        })
        .then((data) => {
          this.showSuggestions = false;
          this.savedValue = data;
          this.handleCityChange = "";
        })
        .catch((error) => {
          console.error("Error al obtener los datos del clima:", error);
        });
    },
    autoLocation() {
      if (this.handleCityChange === "" && !this.isValid) return;

      if (navigator.geolocation) {
        this.isLoading = true;
        navigator.geolocation.getCurrentPosition((position) => {
          this.isCurrentLocation = true;

          fetch(
            `https://api.openweathermap.org/data/2.5/weather?lat=${position.coords.latitude}&lon=${position.coords.longitude}&appid=${this.API_KEY}`,
            {
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
    getSuggestions() {
      if (this.handleCityChange === "") {
        this.showSuggestions = false;
        return;
      }

      if (this.handleCityChange.length < 3) return;

      const service = new window.google.maps.places.AutocompleteService();
      service.getPlacePredictions(
        { input: this.handleCityChange },
        (predictions, status) => {
          if (status === window.google.maps.places.PlacesServiceStatus.OK) {
            this.suggestions = predictions;
            this.showSuggestions = true;
          }
        }
      );
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
#background_weather {
  width: 10em;
  position: absolute;
  top: calc(50% - 4em);
  z-index: -1;
}
.suggestionList {
  position: absolute;
  background-color: white;
  padding: 1em 2em;
  list-style: none;
  top: 25px;
  border-radius: 5px;
}
.suggestionList li {
  padding: 0.5em;
  margin: 0.3em;
  border-bottom: 1px solid #e7e7e7;
  cursor: pointer;
}
.mainContainer_loading {
  display: grid;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.errorText {
  color: red;
  font-size: 1.5em;
  font-weight: bold;
  text-align: center;
  display: grid;
  width: 80%;
  margin: auto;
  margin-top: 2em;
  padding: 0;
}
.errorText span {
  color: #1976d2;
  font-size: 4em;
  font-style: italic;
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
  position: relative;
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
