<template>
  <div class="container">
    <input type="text" v-model="autocomplete.searchText" @keydown="search" />

    <div class="autocomplete" v-if="autocomplete.show">
      <ul>
        <li
          v-for="city in autocomplete.cities"
          @click="selectCity(city)"
          :key="new Date().getTime()"
        >
          <span class="city-name">{{ city.city }}</span>
          <span class="country-name">{{ city.country }}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import CityData from "@/services/CityData.json";
import axios from "axios";

export default {
  data() {
    return {
      autocomplete: {
        searchText: "",
        show: false,
        cities: [],
      },
    };
  },
  inject: ["active", "setActiveWeather"],
  methods: {
    search(e) {
      this.autocomplete.cities = CityData.cities
        .filter(
          (s) =>
            s.city
              .toLowerCase()
              .includes(this.autocomplete.searchText.toLowerCase()) == true
        )
        .sort((a, b) => {
          return parseInt(b.population) - parseInt(a.population);
        })
        .slice(0, 10);

      this.autocomplete.show = true;
    },
    selectCity(city) {
      this.autocomplete.show = false;
      this.autocomplete.searchText = `${city.city}, ${city.country}`;
      axios(
        `https://api.openweathermap.org/data/2.5/onecall?lat=${city.lat}&lon=${city.lng}&exclude=hourly,minutely&units=metric&appid=91a58151e7f031bbef14002c854f771a&cnt=5`
      ).then((resp) => {
        this.setActiveWeather(resp.data);
      });
    },
  },
  created() {
    this.selectCity({
      city: "Istanbul",
      lat: "41.01",
      lng: "28.9603",
      country: "Turkey",
    });
  },
};
</script>

<style lang="scss" scoped>
.container {
  width: 300px;
  max-width: 90%;
  background-color: transparent;
  margin-bottom: 20px;
  border-bottom: 1px solid #fff4;

  input {
    width: 100%;
    border: none;
    padding: 8px 15px;
    background: transparent;
    font-size: 16pt;
    font-weight: 900;
    outline: none;
    color: #fff;
    text-align: center;
    font-family: "Poppins", Inter, -apple-system, BlinkMacSystemFont, "Segoe UI",
      Roboto, Oxygen, Ubuntu, Cantarell, "Fira Sans", "Droid Sans",
      "Helvetica Neue", sans-serif;
    text-shadow: 1px 1px 0 rgba(0, 0, 0, 0.5);
  }

  div.autocomplete {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    z-index: 20;
    transform: translateY(calc(100% + 10px));
    background: rgba(255, 255, 255, 1);
    border-radius: 2px;
    padding: 10px 0;
    overflow: hidden;
    box-shadow: 1px 1px 1px 0px rgba(0, 0, 0, 0.2);

    ul {
      margin: 0;
      padding: 0;
      list-style: none;
      width: 100%;
      overflow: hidden;

      li {
        display: flex;
        flex-direction: row;
        width: 100%;
        padding: 5px 10px;
        border-bottom: 1px solid #f1f1f1;
        white-space: nowrap;
        text-overflow: ellipsis;
        overflow: hidden;
        cursor: pointer;

        &:hover {
          background: #f1f1f1;
        }

        &:last-child {
          border-bottom: 0px;
        }

        span.city-name {
          display: inline-block;
          font-size: 13pt;
          text-overflow: ellipsis;
          white-space: nowrap;
          text-overflow: ellipsis;
          overflow: hidden;
          &::after {
            content: ",";
          }
        }

        span.country-name {
          display: inline-block;
          margin-left: 5px;
          font-size: 13pt;
          text-overflow: ellipsis;
          flex: 1 0 auto;
          width: 10px;
          white-space: nowrap;
          text-overflow: ellipsis;
          overflow: hidden;
        }
      }
    }
  }
}
</style>
