<template>
  <main
    class="w-4/6 bg-cerulean-100 transform rotate-6 mx-24 -mb-20 -mt-20 flex justify-center items-center"
  >
    <div class="transform -rotate-6 text-2xl flex justify-center items-center">
      <canvas
        ref="canvas"
        :width="width"
        :height="height"
        @mousemove="onMouseMove"
        @mouseleave="clearArea"
        :class="['bg-white shadow-2xl', inArea && 'cursor-pointer']"
      ></canvas>
    </div>
  </main>
</template>

<script>
export default {
  name: "Preview",
  data() {
    return {
      width: 600,
      height: 600,
      canvas: null,
      ctx: null,
      inArea: false
    };
  },
  methods: {
    drawBackgroundImage() {
      return new Promise(resolve => {
        const backgroundImage = new Image();
        backgroundImage.src = require("@/assets/drake-meme.jpg");
        backgroundImage.onload = () => {
          this.ctx.drawImage(backgroundImage, 0, 0, this.width, this.height);
          resolve();
        };
      });
    },
    drawArea() {
      const color = "white";

      this.drawAreaOne(color);
      this.drawAreaTwo(color);
    },
    drawAreaOne(color) {
      this.ctx.fillStyle = color;
      this.ctx.fillRect(this.width / 2, 0, this.width / 2, this.height / 2);
    },
    drawAreaTwo(color) {
      this.ctx.fillStyle = color;
      this.ctx.fillRect(
        this.width / 2,
        this.width / 2,
        this.width / 2,
        this.height / 2
      );
    },
    clearAreaOne() {
      this.ctx.clearRect(this.width / 2, 0, this.width / 2, this.height / 2);
    },
    clearAreaTwo() {
      this.ctx.clearRect(
        this.width / 2,
        this.width / 2,
        this.width / 2,
        this.height / 2
      );
    },
    onMouseMove(event) {
      const rect = this.canvas.getBoundingClientRect();
      const scaleX = this.canvas.width / rect.width;
      const scaleY = this.canvas.height / rect.height;
      const x = (event.clientX - rect.left) * scaleX;
      const y = (event.clientY - rect.top) * scaleY;
      const color = "#f1f1f1";

      if (x >= this.width / 2 && y <= this.height / 2) {
        this.drawAreaOne(color);
        this.inArea = true;
      } else if (x >= this.width / 2 && y >= this.height / 2) {
        this.drawAreaTwo(color);
        this.inArea = true;
      } else if (x <= this.width / 2) {
        this.clearArea();
        this.inArea = false;
      }

      if (y >= this.height / 2) {
        this.clearAreaOne();
      } else if (y <= this.height / 2) {
        this.clearAreaTwo();
      }
    },
    clearArea() {
      this.clearAreaOne();
      this.clearAreaTwo();
    }
  },
  mounted() {
    // init canvas
    this.canvas = this.$refs.canvas;
    this.ctx = this.canvas.getContext("2d");

    // set background image
    this.drawBackgroundImage().then(() => {
      this.drawArea();
    });
  }
};
</script>
