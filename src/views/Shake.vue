<template>
  <div class='home'>
    <ShakeVideo />
    <iframe src="https://player.vimeo.com/video/678926274?h={hash_parameter}" width="{video_width}" height="{video_height}" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>
    <div id="vim1"></div>
  </div>
</template>

<script src="https://player.vimeo.com/api/player.js"></script>
<script>
// @ is an alias to /src
import ShakeVideo from '@/components/ShakeVideo.vue';
import axios from 'axios';

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
    axios.get('http://localhost/shakes').then((response) => {
      console.log(response);
    });
    document.getElementById('videoFront1').play();
    //Vimeo
    const options01 = {
      id: 678926274,
      width: 500
    };
    const video01Player = new Vimeo.Player('vim1', options01);
    video01Player.on('play', function() {
      console.log('Played the first video');
    });
  },
};
</script>