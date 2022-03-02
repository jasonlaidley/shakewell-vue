<template>
  <div class='home'>
    <h1>{{ timerCount }}</h1>
  </div>
</template>

<script>
export default {
  name: 'Countdown',
  props: {
    timerCount: Number,
  },
};
</script>
