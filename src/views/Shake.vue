<template>
  <div class='home'>
    <img alt='Vue logo' src='../assets/logo.png' />
    <ShakeVideo />
  </div>
</template>

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
    console.log('hello');
    axios.get('http://localhost/shakes').then((response) => {
      console.log(response);
    });
    document.getElementById('videoFront1').play();
  },
};
</script>
