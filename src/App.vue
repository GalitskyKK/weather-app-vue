<template>
  <div class="weather">
    <date-time />
    <div class="container">
      <weather-form
        :searchQuery="searchQuery"
        @update:searchQuery="searchQuery = $event"
        @search="weatherSearch" />

      <div class="card weather-load" v-if="loading">Loading...</div>

      <weather-info
        v-show="!error && location && temperature !== 0 && description"
        :location="location"
        :temperature="temperature"
        :description="description"
        :descriptionIcon="descriptionIcon"
        :error="error" />
    </div>

    <div class="search-history" v-if="searchHistory.length">
      <h3 @click="toggleHistory">{{ showHistory ? 'Hide history ↑' : 'Show history ↓' }}</h3>
      <ul v-show="showHistory">
        <li v-for="city in searchHistory" :key="city">
          <button @click="searchFromHistory(city)">{{ city }}</button>
        </li>
      </ul>
      <button @click="clearSearchHistory" class="clear-history-button">Clear History</button>
    </div>

    <div class="weather-bg" :class="weatherClass">
      <div></div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import dateTime from './components/date-time.vue';
import WeatherForm from './components/weather-form.vue';
import WeatherInfo from './components/weather-info.vue';

export default defineComponent({
  components: { dateTime, WeatherForm, WeatherInfo },
  name: 'App',
  data() {
    return {
      location: '',
      temperature: 0,
      description: '',
      descriptionIcon: '',
      searchQuery: '',
      loading: false,
      error: false,
      searchHistory: [] as string[],
      showHistory: false,
    };
  },
  computed: {
    weatherClass() {
      return this.description.toLowerCase().replace(/\s+/g, '-');
    }
  },
  methods: {
    weatherSearch() {
      if (this.searchQuery.trim() === '') return; // Проверка на пустую строку

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
          this.descriptionIcon = `https:${data.current.condition.icon}`;
          this.addToSearchHistory(this.searchQuery);
          this.resetSearchQuery();
        })
        .catch((error) => {
          this.loading = false;
          this.error = true;
          console.error(error);
        });
    },
    addToSearchHistory(city: string) {
      if (!this.searchHistory.includes(city)) {
        this.searchHistory.push(city);
        localStorage.setItem('weatherSearchHistory', JSON.stringify(this.searchHistory));
      }
    },
    loadSearchHistory() {
      const history = localStorage.getItem('weatherSearchHistory');
      if (history) {
        this.searchHistory = JSON.parse(history);
      }
    },
    searchFromHistory(city: string) {
      this.searchQuery = city;
      this.weatherSearch();
    },
    resetSearchQuery() {
      this.searchQuery = '';
    },
    toggleHistory() {
      this.showHistory = !this.showHistory;
    },
    clearSearchHistory() {
      this.searchHistory = [];
      localStorage.removeItem('weatherSearchHistory');
    }
  },
  mounted() {
    this.loadSearchHistory(); // Загрузка истории поиска при инициализации компонента
  }
});
</script>

<style lang="scss" scoped>
.search-history {
  display: flex;
  flex-direction: column;
  background-color: #ffffff75;
  border-radius: 10px;
  padding: 20px 30px;
  box-shadow: 1px 1px 20px #00000015;
}

.search-history ul {
  list-style-type: none;
  padding: 0;
}

.search-history li {
  text-decoration: none;
}

.search-history button {
  text-transform: capitalize;
  color: #313130;
  text-decoration: none;
  background: none;
  border: none;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  text-decoration: underline;
}

.clear-history-button {
  margin-top: 10px;
  color: red;
  background: none;
  border: none;
  font-size: 14px;
  cursor: pointer;
  text-decoration: underline;
}
</style>
