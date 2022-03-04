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
    exerciseDeets: Object
  },
  data() {
    return {
      videoPlayer: {},
    };
  },
  watch: {
    // whenever 'playing' changes, this function will run
    'exerciseDeets.playing'(playing) {
      //If playing turns to true 
      if (playing === true) {
        this.playVideo();
      } else {
        //
      }
    }
  },
  methods: {
    loadVideo() {
      //Get Vimeo video id from schedule
      const videoId = this.exerciseDeets.video_side;
      
      if (videoId > 0) {
        //Vimeo options
        const videoOptions = {
          id: videoId,
          width: 500,
          height: 500,
          controls: false,
          keyboard: false,
          muted: true,
          title: false,
          autopause: false
        };
        
        //Vimeo player
        const videoPlayer = new Player(videoId, videoOptions);
        this.videoPlayer = videoPlayer;
      }
    },
    playVideo() {
      //Play
      this.videoPlayer.play();
    }
  },
  mounted() {
    this.loadVideo();
    
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
