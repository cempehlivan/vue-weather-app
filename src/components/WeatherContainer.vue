<template>
  <div
    class="container"
    v-bind:class="{
      'animate__animated animate__fadeInUp':addClass,
    }"
  >
    <WeatherDailyContainer />
  </div>
</template>

<script>
import WeatherDailyContainer from "@/components/WeatherDailyContainer.vue";
export default {
  components: {
    WeatherDailyContainer,
  },
  inject: ["active"],
  watch: {
   active: {
    handler(){
        this.addClass = true;
        setTimeout(()=> {
            this.addClass = false;
        }, 1000);
    },
    deep: true
   }
  },
  data(){
    return {
        addClass: false
    }
  }
};
</script>

<style lang="scss" scoped>
div.container {
  width: 100%;
  max-width: 90%;
  border-radius: 20px;
  padding: 15px;
  z-index: 10;
  transition: all 0.2s ease-in-out;
}
</style>
