<template>
  <div v-if="exerciseDeets.video_side" :id="divId" :class="['videoDiv', exerciseDeets.playing ? 'showVideo' : 'hideVideo']"></div>
</template>

<script>
// @ is an alias to /src
import Player from '@vimeo/player';

export default {
  name: 'Exercises',
  props: {
    divId: String,
    exerciseDeets: Object,
    videoSize: Number,
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
          width: this.videoSize,
          height: this.videoSize,
          controls: false,
          keyboard: false,
          muted: true,
          title: false,
          autopause: false
        };
        
        //Vimeo player
        const videoPlayer = new Player(this.divId, videoOptions);
        this.videoPlayer = videoPlayer;
      }
    },
    playVideo() {
      const videoId = this.exerciseDeets.video_side;
      //Play
      this.videoPlayer.play()
      .then(function() {
        console.log(`playing ${videoId}`);
      }).catch(function(error) {
        console.log(error);
      });
    }
  },
  mounted() {
    this.loadVideo();
    
    //Play
    /*this.videoPlayer.play()
    .then(function() {
      console.log("The video is playing");
    }).catch(function(error) {
      console.log(error);
    });*/

    //Seek
    /*this.videoPlayer.setCurrentTime(10)
    .then(function(seconds) {
      console.log(`actual seek ${seconds}`)
    }).catch(function(error) {
        console.log(error);
    });*/

    //Progress - how nmuch is loaded?
    /*this.videoPlayer.on('progress', function(data) {
      console.log(`progress: ${JSON.stringify(data)}`);
    });*/

    //Timeupdate - current time
    /*this.videoPlayer.on('timeupdate', function(data) {
      console.log(`timeupdate: ${JSON.stringify(data)}`);
    });*/
  },
};
</script>

<style lang="scss">
#videoDiv {

  .videoDiv {
    position: absolute;
    top: 0;
    left: 0;
  }

  .hideVideo {
    opacity: 0;
  }

  .showVideo {
    animation-name: fade-in;
    animation-duration: 5s;
  }

  @keyframes fade-in {
    0% { opacity: 0; }
    100% { opacity: 1; }
  }
}
</style>