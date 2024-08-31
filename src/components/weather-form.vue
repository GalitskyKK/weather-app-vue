<template>
  <div class="card weather-form">
    <input
      type="text"
      class="weather-form__input"
      :value="searchQuery"
      @input="$emit('update:searchQuery', $event.target.value)"
      @keyup.enter="search"
      placeholder="Enter city" />
    <button class="weather-form__button" @click="search">Search</button>
  </div>
</template>

<script>
export default {
  props: {
    searchQuery: {
      type: String,
      required: true
    }
  },
  methods: {
    search() {
      this.$emit('search');
    }
  }
};
</script>

<style lang="scss" scoped>

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

.weather-form {
  display: flex;
  align-items: stretch;
  gap: 20px;

  &__input {
    flex-grow: 1;
    font-size: 20px;
    border: 1px solid #cccccc;
    border-radius: 10px;
    transition: all 0.3s;
    padding: 10px 20px;

    &:focus {
      outline: none;
      border-color: #99b1dd;
    }
  }

  &__button {
    flex-basis: 180px;
    font-size: 20px;
    border: 1px solid #cccccc;
    background-color: #ffffff;
    border-radius: 10px;
    padding: 10px 20px;
    transition: all 0.3s;
    cursor: pointer;

    &:hover {
      background-color: #eff5ff;
    }
  }
}

@media (max-width: 768px) {
  .weather-form {

    &__button {
      flex-basis: 0;
    }
  }
}

@media (max-width: 590px) {
  .weather-form {
    flex-direction: column;

    &__button {
      flex-basis: 0;
    }
  }
}

</style>