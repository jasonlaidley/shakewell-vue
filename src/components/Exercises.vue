<template>
  <div class='home'>
    <div v-if="exerciseDeets.video_side" :id="exerciseDeets.video_side"></div>
    {{ exerciseDeets }}
  </div>
</template>

<script>
// @ is an alias to /src
import Player from '@vimeo/player';

export default {
  name: 'Exercises',
  props: {
    exerciseDeets: [Array, Object],
  },
  mounted() {
    //console.log( this.exerciseDeets.video_side );
    let vidId = this.exerciseDeets.video_side;
    if (this.exerciseDeets.video_side > 0) {
      //Vimeo
      let options01 = {
        id: vidId,
        width: 500,
        height: 500,
        controls: false,
        keyboard: false,
        muted: true,
        title: false,
        autopause: false
      };
      let player1 = new Player(vidId, options01);
    }
    
    //Play
    /*player1.play()
    .then(function() {
      console.log("The video is playing");
    }).catch(function(error) {
      console.log(error);
    });*/
    //Seek
    /*player1.setCurrentTime(10)
    .then(function(seconds) {
      console.log(`actual seek ${seconds}`)
    }).catch(function(error) {
        console.log(error);
    });*/
    //Progress - how nmuch is loaded?
    /*player1.on('progress', function(data) {
      console.log(`progress: ${JSON.stringify(data)}`);
    });*/
    //Timeupdate - current time
    /*player1.on('timeupdate', function(data) {
      console.log(`timeupdate: ${JSON.stringify(data)}`);
    });*/
  },
};
</script>
