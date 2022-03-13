<template>
  <div>
    <Countdown :timerCount="timerCount"/>
    <div id="listDiv">
      <ExerciseList
        v-for="(video, index) in previewVideos"
        :key="index"
        :divId="index"
        :exerciseDeets="video"
      ></ExerciseList>
    </div>
    <div id="videoDiv">
      <Exercises
        v-for="(video, index) in mainVideos"
        :key="index"
        :divId="index"
        :exerciseDeets="video"
        :videoSize="500"
      ></Exercises>
    </div>
    <Conference id="conferenceDiv"/>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios';
import Countdown from '@/components/Countdown.vue';
import ExerciseList from '@/components/ExerciseList.vue';
import Exercises from '@/components/Exercises.vue';
import Conference from '@/components/Conference.vue';

export default {
  name: 'Home',
  components: {
    Countdown,
    ExerciseList,
    Exercises,
    Conference,
  },
  data() {
    return {
      schedule: {},
      previewVideos: {},
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

    // Create video list
    createVideoList() {
      //Create temp video objects
      let previewVideosArray = {};
      let mainVideosArray = {};

      //Go through each second on schedule
      this.schedule.forEach(function(scheduled, index) {
        
        //If scheduled item contains a 'preview' event
        if (`preview${index}` in scheduled) {
          //Create video object
          let previewVideoKey = `preview${index}`;
          let previewVideoObject = {[previewVideoKey]: scheduled[previewVideoKey]};
          //Assign to temp object
          previewVideosArray = Object.assign(previewVideosArray, previewVideoObject);
        }

        //If scheduled item contains a 'main' event
        if (`main${index}` in scheduled) {
          //Create video object
          let mainVideoKey = `main${index}`;
          let mainVideoObject = {[mainVideoKey]: scheduled[mainVideoKey]};
          //Assign to temp object
          mainVideosArray = Object.assign(mainVideosArray, mainVideoObject);
        }
      });

      //Update stored data
      this.previewVideos = previewVideosArray;
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
      let previewVideoKey = `preview${second}`;
      if (this.schedule[second][previewVideoKey]) {
        this.schedule[second][previewVideoKey]['playing'] = true;
      }
    },
  },
  mounted() {
    //Get schedule from server
    this.getSchedule()
    .then(
      (schedule) => {
        this.createVideoList();
        this.timer(schedule);
      }
    );
  }
};
</script>

<style lang="scss">
#listDiv {
  display: inline-block;
  vertical-align: top;
  width: 20%;
}

#videoDiv {
  position: relative;
  display: inline-block;
  vertical-align: top;
  width: 60%;
}

#conferenceDiv {
  display: inline-block;
  vertical-align: top;
  width: 20%;
}
</style>
