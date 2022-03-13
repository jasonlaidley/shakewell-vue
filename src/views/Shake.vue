<template>
  <div class='home'>
    <!--<ShakeVideo />-->
    <div id="vim1"></div>
  </div>
</template>

<script src="https://player.vimeo.com/api/player.js"></script>
<script>
// @ is an alias to /src
import Player from '@vimeo/player';

export default {
  data() {
    return {
      timerCount: 30,
    };
  },
  name: 'Home',
  components: {
    ShakeVideo,
  },
  mounted() {
    //Vimeo
    const options01 = {
      id: 678926305,
      width: 500,
      height: 500,
      controls: false,
      keyboard: false,
      muted: true,
      title: false,
      autopause: false
    };
    const player1 = new Player('vim1', options01);
    //Play
    player1.play()
    .then(function() {
      console.log("The video is playing");
    }).catch(function(error) {
      console.log(error);
    });
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
