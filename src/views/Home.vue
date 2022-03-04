<template>
  <div class="home">
    <Countdown :timerCount="timerCount"/>
    <Exercises
      v-for="(item, index) in mainVideos"
      :key="index"
      :exerciseDeets="item"
    ></Exercises>
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
      mainVideos: {},
      timerCount: 0,
    };
  },
  methods: {
    //Get schedule from server
    async getSchedule() {  
      try {
        // Call api
        const { data: schedule } = await axios.get('http://localhost/schedule');
        console.log(schedule);
        this.schedule = schedule;
        return schedule;
      // Error
      } catch (error) {
        console.error(error);
      }
    },

    // Creeate video list
    updateVideoList() {
      //Go through each second on schedule
      let mainVideosArray = {};
      this.schedule.forEach(function(scheduled, index) {
        let videoKey = `main${index}`;
        //If scheduled item contains a 'main' event
        if (videoKey in scheduled) {
          //Create video object
          let mainVideoObj = {[videoKey]: scheduled[videoKey]};
          //Assign to object
          mainVideosArray = Object.assign(mainVideosArray, mainVideoObj);
        }
      });
      //Update stored data
      this.mainVideos = mainVideosArray;
    },

    //Go through each item on schedule once per second
    timer(schedule) {
      //Perfom every second until end of schedule
      let second = 0;
      const timerId = setInterval(() => {
        //Run schedule check
        this.checkSchedule(second);
        //End
        if (second === 30) { clearInterval(timerId); }
        //Accumulate second
        second += 1;
      }, 1000);
    },

    //Check what events there are every second
    checkSchedule(second) {
      //Time
      console.log(second);

      //Exercise remaining time countdown
      let countdownKey = `countdown${second}`;
      if (this.schedule[second][countdownKey]) {
        //Update timerCoont data
        let countdown = this.schedule[second][countdownKey]['display'];
        this.timerCount = countdown;
      }

      //Main exercise video
      let mainVideoKey = `main${second}`;
      if (this.schedule[second][mainVideoKey]) {
        this.schedule[second][mainVideoKey]['playing'] = true;
      }

      //Preview exercise video
      if (this.schedule[second]['preview']) {
        //console.log(`preview: ${schedule[second]["preview"]["video_front"]}`);
      }
    },

    //Play video
    startPlaying(second) {
      const videoToPlay = this.schedule.filter(element => {
        return element['main']['id'] == second;
      });
      console.log(videoToPlay);
      //this.schedule = second;
    }
  },
  mounted() {
    //Get schedule from server
    this.getSchedule()
    .then(
      (schedule) => {
        this.updateVideoList();
        this.timer(schedule);
      }
    );
  }
};
</script>
