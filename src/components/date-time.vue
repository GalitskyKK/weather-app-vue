<template>
  <div class="clock-container">
    <!-- Часы, минуты и секунды большими цифрами -->
    <div class="time">
      <span class="hours">{{ hours }}:</span>
      <span class="minutes">{{ minutes }}:</span>
      <span class="seconds">{{ seconds }}</span>
    </div>
    
    <!-- День недели, число и месяц меньшим шрифтом -->
    <div class="date">
      <span class="weekday">{{ weekday }}</span>,
      <span class="day">{{ day }}</span> 
      <span class="month">{{ month }}</span>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentTime: new Date(),
    };
  },
  computed: {
    hours() {
      return this.currentTime.getHours().toString().padStart(2, '0');
    },
    minutes() {
      return this.currentTime.getMinutes().toString().padStart(2, '0');
    },
    seconds() {
      return this.currentTime.getSeconds().toString().padStart(2, '0');
    },
    weekday() {
      const options = { weekday: 'long' };
      return this.currentTime.toLocaleDateString('en-EN', options);
    },
    day() {
      return this.currentTime.getDate();
    },
    month() {
      const options = { month: 'long' };
      return this.currentTime.toLocaleDateString('en-En', options);
    },
  },
  methods: {
    updateTime() {
      setInterval(() => {
        this.currentTime = new Date();
      }, 1000);
    },
  },
  mounted() {
    this.updateTime();
  },
};
</script>

<style scoped>
.clock-container {
  text-align: center;
  color: #313130;
}

.time {
  font-size: 68px;
  font-weight: 400;
}

.date {
  font-size: 24px;
  font-weight: bold;
}

.weekday,
.day,
.month {
  margin: 0 5px;
}
</style>
