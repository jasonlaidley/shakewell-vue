<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue';
import axios from 'axios';

export default {
  name: 'Home',
  components: {
    HelloWorld,
  },
  mounted() {
    // Get exercise list
    async function getExercises() {
      // check Schedule
      const checkSchedule = (seconds) => {
        console.log(seconds);
      };

      // Timer
      const timer = () => {
        let seconds = 0;
        // Each second
        const timerId = setInterval(() => {
          checkSchedule(seconds);
          // End
          if (seconds === 5) { clearInterval(timerId); }
          seconds += 1;
        }, 1000);
      };

      // Make schedule
      const makeSchedule = (exercises) => {
        // Add start time to each exercise
        let startSeconds = 0;
        let accumulatedSeconds = 0;
        const schedule = exercises.map((exercise) => {
          startSeconds = accumulatedSeconds;
          accumulatedSeconds += exercise.exercise_duration;
          return {
            ...exercise,
            startTime: startSeconds,
          };
        });
        console.log(schedule);
      };

      try {
        // Call api
        const { data: exercises } = await axios.get('http://localhost/exercises');
        console.log(exercises);

        // Make schedule
        makeSchedule(exercises);

        // Start timer
        timer();

      // Error
      } catch (error) {
        console.error(error);
      }
    }
    getExercises();
  },
};
</script>
