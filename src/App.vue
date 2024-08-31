<template>
  <div class="weather">
    <date-time/>
    <div class="container">
      <div class="card weather-form">
        <input
          type="text"
          class="weather-form__input"
          v-model="searchQuery"
          @keyup.enter="weatherSearch"
          placeholder="Enter city" />
        <button class="weather-form__button" @click="weatherSearch">Search</button>
      </div>

      <div class="card weather-load" v-if="loading">Loading...</div>

      <div class="weather-info" v-show="!error && location && temperature !== 0 && description">
        <div class="card" v-if="error">Error</div>
        <div class="weather-info__title">
          <p class="card">{{ location }}</p>
          <p class="card">{{ temperature }} °C</p>
          <p class="card">{{ description }}</p>
        </div>
      </div>
    </div>
    <div class="weather-bg" :class="weatherClass">
      <div></div>
    </div>
  </div>
</template>

<script>
import dateTime from './components/date-time.vue';
export default {
  components: { dateTime },
  name: 'App',
  data() {
    return {
      location: '',
      temperature: 0,
      description: '',
      searchQuery: '',
      loading: false,
      error: false,
      currentTime: new Date().toLocaleString(),
    };
  },
  computed: {
    weatherClass() {
      return this.description.toLowerCase().replace(/\s+/g, '-')
    }
  },
  methods: {
    weatherSearch() {
      this.loading = true;
      this.error = false;
      fetch(
        `https://api.weatherapi.com/v1/current.json?key=1dd2ada5141d4c90af3213338242908&q=${this.searchQuery}&lang=en`,
      )
        .then((response) => response.json())
        .then((data) => {
          this.loading = false;
          this.location = data.location.name;
          this.temperature = data.current.temp_c;
          this.description = data.current.condition.text;
          this.resetSearchQuery();
        })
        .catch((error) => {
          this.loading = false;
          this.error = true;
          console.error(error);
        });
    },
    resetSearchQuery() {
      this.searchQuery = '';
    },
    updateTime() {
      setInterval(() => {
        this.currentTime = new Date();
      }, 1000);
    },
  },
  mounted() {
    this.updateTime();
  }
};
</script>

<style lang="scss" scoped>

</style>
