<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <Exercises
      v-for="(item, index) in schedule"
      :item="item"
      :key="index"
      :exerciseDeets="item.main"
    ></Exercises>
    <Countdown :timerCount="timerCount"/>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios';
import Exercises from '@/components/Exercises.vue';
import Countdown from '@/components/Countdown.vue';

export default {
  name: 'Home',
  components: {
    Exercises,
    Countdown,
  },
  data() {
    return {
      schedule: {},
      timerCount: 0,
    };
  },
  methods: {
    increment(seconds) {
      this.timerCount = seconds;
    }
  },
  mounted() {
    // check Schedule
    const checkSchedule = (seconds, schedule) => {
      //Time
      console.log(schedule[seconds]['time']['start_seconds']);
      //Exercise remaining time countdown
      if (schedule[seconds]['countdown']) {
        //console.log(`countdown: ${schedule[seconds]["countdown"]["display"]}`);
        let countdown = schedule[seconds]["countdown"]["display"];
        this.timerCount = countdown;
        //this.increment(seconds)
      }
      //Main exercise video
      if (schedule[seconds]['main']) {
        //console.log(`main: ${schedule[seconds]["main"]["video_front"]}`);
      }
      //Preview exercise video
      if (schedule[seconds]['preview']) {
        //console.log(`preview: ${schedule[seconds]["preview"]["video_front"]}`);
      }
    };

    //Make Video list
    const makeVideoList = (schedule) => {
      const result = schedule.filter(element => {
        return element['main'];
      });
      this.schedule = result;
    }

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

    // Get schedule list
    const getSchedule = async () => {  
      try {
        // Call api
        const { data: schedule } = await axios.get('http://localhost/schedule');
        //this.schedule = schedule;
        return schedule;

      // Error
      } catch (error) {
        console.error(error);
      }
    }

    getSchedule()
    .then(
      (schedule) => {
        makeVideoList(schedule);
        timer(schedule);
      }
    );
  },
};
</script>
