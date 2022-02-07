<template>
    <div class="edgeHideFront">
        <video id="videoFront1" class="vidFront" muted preload="auto"  poster="http://localhost/storage/app/media/whiteSquare.png">
            <source src="http://localhost/storage/app/media/videos/ben_00_armCircles_front.mp4" type="video/mp4">
            <source src="http://localhost/storage/app/media/webm/ben_00_armCircles_front.webm" type="video/webm">
        </video>
    </div>
</template>

<script>
export default {
  name: 'ShakeVideo',
  props: {
    msg: String,
  },
};
</script>

<style scoped lang="scss">
.edgeHideFront {
  width: 300px;
  height: 300px;
  /*position: absolute;
  width: 50%;
  top: 50vh;
  left: 2.5%;
  transform: translateY(-50%);
  overflow: hidden;*/
}
.vidFront {
  position: relative;
  width: 100%;
  /*
    top: 50%;
    left: 2.5%;
    transform: translateY(-50%);
    */
  /*
    position: absolute;
    top: -1%;
    left: -1%;
    width: 51%;
    border: 2px solid red;
    display: none;
    */
}
</style>
