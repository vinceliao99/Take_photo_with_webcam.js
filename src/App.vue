<template>
  <div>
    <video id="webcam" autoplay playsinline width="1920" height="1280"></video>
  </div>
  <div>
    <button @click="snap">Snap</button>
  </div>
  <div>
    <canvas id="canvas" class="d-none"></canvas>
  </div>
</template>

<script>
import Webcam from "./webcam";
export default {
  name: "App",
  data: function () {
    return {
      webcam: null,
    };
  },
  components: {},
  mounted() {
    const webcamElement = document.getElementById("webcam");
    const canvasElement = document.getElementById("canvas");
    const webcam = new Webcam(webcamElement, "enviroment", canvasElement);
    this.webcam = webcam;

    //webcam.facingMode = { exact: 'environment' };

    webcam
      .start()
      .then((result) => {
        console.log("webcam started");
      })
      .catch((err) => {
        console.log(err);
      });
  },
  methods: {
    snap() {
      var picture = this.webcam.snap();
      this.downloadImage(picture, "mySnap.jpeg");
    },
    // Save | Download image
    downloadImage(data, filename = "untitled.jpeg") {
      var a = document.createElement("a");
      a.href = data;
      a.download = filename;
      document.body.appendChild(a);
      a.click();
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
