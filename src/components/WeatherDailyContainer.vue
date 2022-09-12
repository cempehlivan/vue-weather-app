<template>
  <div class="daily-container">
    <ul>
      <li v-for="item in active.weather.daily" :key="new Date().getTime()">
        <WeatherIcon :icon="item.weather[0].icon" />
        <span class="cl">{{ Math.ceil(item.temp.max) }}</span>
        <span class="description">{{ item.weather[0].description }}</span>
        <span class="date">{{
          new Date(item.dt * 1000).toLocaleDateString()
        }}</span>
      </li>
    </ul>
  </div>
</template>

<script>
import WeatherIcon from "./WeatherIcon.vue";
export default {
  inject: ["active"],
  components: {
    WeatherIcon,
  },
};
</script>

<style lang="scss" scoped>
.daily-container {
  display: block;
  width: 100%;

  ul {
    margin: 0;
    padding: 0;
    list-style: none;
    display: flex;
    flex-direction: row;
    column-gap: 10px;
    justify-content: center;

    li {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      flex: 1;
      background: rgba(255, 255, 255, 0.5);
      border-radius: 15px;
      padding: 5px 10px;
      max-width: 140px;
      cursor: pointer;
      transition: all .2s ease-in-out;

      &:hover {
        background: #fff;
        transform: scale(1.2);
      }

      span.cl {
        font-weight: 900;
        font-size: 16pt;
        display: inline-block;
        position: relative;

        &::after {
          content: "°C";
          position: absolute;
          right: 0;
          top: 0;
          font-size: 11pt;
          transform: translateX(100%);
          font-weight: 400;
        }
      }

      span.description {
        font-weight: 600;
        display: inline-block;
        text-align: center;
      }

      span.date {
        font-size: 11pt;
        font-weight: 400;
      }
    }
  }
}
</style>
