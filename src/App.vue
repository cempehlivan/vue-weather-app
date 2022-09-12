<template>
  <SearchBox />
  <WeatherContainer />
</template>

<script>
import SearchBox from "@/components/SearchBox.vue";
import WeatherContainer from "@/components/WeatherContainer.vue";

export default {
  components: {
    SearchBox,
    WeatherContainer,
  },
  data() {
    return {
      active: {
        weather: {},
      },
    };
  },
  methods: {
    setActiveWeather(data) {
      this.active.weather = data;
    },
  },
  provide() {
    return {
      active: this.active,
      setActiveWeather: this.setActiveWeather,
    };
  },
};
</script>

<style>
div#app {
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
</style>
