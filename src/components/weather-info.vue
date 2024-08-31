<template>
  <div class="weather-info" v-show="!error && location && temperature !== 0 && description">
    <div class="card" v-if="error">Error</div>
    <div class="weather-info__title">
      <p class="card">{{ location }}</p>
      <p class="card">{{ temperature }} °C</p>
      <p class="card">
        {{ description }}
        <img
          v-if="descriptionIcon"
          :src="descriptionIcon"
          alt="Weather Icon"
          class="weather-icon" />
      </p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'WeatherInfo',
  props: {
    location: {
      type: String,
      required: true
    },
    temperature: {
      type: Number,
      required: true
    },
    description: {
      type: String,
      required: true
    },
    descriptionIcon: {
      type: String,
      required: false
    },
    error: {
      type: Boolean,
      required: true
    }
  }
});
</script>

<style scoped lang="scss"> 

.weather-info {
  &__title {
    display: grid;
    grid-template-columns: 1fr auto auto;
    gap: 20px;
    font-size: 40px;
  }
}

.card {
  display: flex;
  align-items: center;
  place-items: center;
  justify-content: center;
  background-color: #ffffff;
  border-radius: 10px;
  padding: 20px 30px;
  box-shadow: 1px 1px 20px #00000015;
}

@media (max-width: 768px) {

  .weather-info__title {
    font-size: 30px;
    gap: 10px;
    grid-template-columns: max-content auto auto;
  }
}

@media (max-width: 590px) {

  .weather-info__title {
    font-size: 30px;
    gap: 10px;
    grid-template-columns: auto;
  }
}

@media (max-width: 425px) {

  .card {
    padding: 10px 20px;
  }

  .weather-info__title {
    font-size: 30px;
    gap: 10px;
    grid-template-columns: auto;
  }
}

</style>
