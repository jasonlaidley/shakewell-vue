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
    // check Schedule
    const checkSchedule = (seconds, schedule) => {
      //Time
      console.log(schedule[seconds]['time']['start_seconds']);
      //Exercise remaining time countdown
      if (schedule[seconds]['countdown']) {
        console.log(`countdown: ${schedule[seconds]["countdown"]["display"]}`);
      }
      //Main exercise video
      if (schedule[seconds]['main']) {
        console.log(`main: ${schedule[seconds]["main"]["video_front"]}`);
      }
      //Preview exercise video
      if (schedule[seconds]['preview']) {
        console.log(`preview: ${schedule[seconds]["preview"]["video_front"]}`);
      }
    };

    // Timer
    const timer = (schedule) => {

      console.log(schedule);
      let seconds = 0;

      // Each second
      const timerId = setInterval(() => {
        // Run schedule check
        checkSchedule(seconds, schedule);
        // End
        if (seconds === 30) { clearInterval(timerId); }
        // Accumulate seconds
        seconds += 1;
      }, 1000);
    };

    // Get exercise list
    const getSchedule = async () => {  
      try {
        // Call api
        const { data: schedule } = await axios.get('http://localhost/schedule');
        return schedule;

      // Error
      } catch (error) {
        console.error(error);
      }
    }

    getSchedule()
    .then(
      (schedule) => {
        timer(schedule)
      }
    );
  },
};
</script>
