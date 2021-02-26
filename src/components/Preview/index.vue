<template>
  <main
    class="w-4/6 bg-cerulean-100 transform rotate-6 ml-20 mr-24 -mb-20 -mt-20 flex justify-center items-center"
  >
    <div class="transform -rotate-6 text-2xl flex justify-center items-center">
      <canvas
        ref="canvas"
        :width="width"
        :height="height"
        :class="['bg-white shadow-2xl', currentArea !== 0 && 'cursor-pointer']"
        @mousemove="onMouseMove"
        @mouseleave="clearArea"
        @click="selectArea"
      ></canvas>
    </div>
  </main>
</template>

<script>
import eventBus from "@/eventBus";

export default {
  name: "Preview",
  data() {
    return {
      width: 600,
      height: 600,
      canvas: null,
      ctx: null,
      currentArea: 0,
      selectedArea: 0
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
      const fillText = false;

      this.drawAreaOne(color, fillText);
      this.drawAreaTwo(color, fillText);
    },
    drawAreaOne(color, fillText) {
      this.ctx.fillStyle = color;
      this.ctx.fillRect(this.width / 2, 0, this.width / 2, this.height / 2);

      if (fillText) {
        this.ctx.textAlign = "center";
        this.ctx.textBaseline = "middle";
        this.ctx.font = "30px Architects Daughter";
        this.ctx.fillStyle = "#000";

        this.ctx.fillText(
          "Area 1",
          this.width / 2 + this.width / 2 / 2,
          this.height / 2 / 2
        );
      }
    },
    drawAreaTwo(color, fillText) {
      this.ctx.fillStyle = color;
      this.ctx.fillRect(
        this.width / 2,
        this.width / 2,
        this.width / 2,
        this.height / 2
      );

      if (fillText) {
        this.ctx.textAlign = "center";
        this.ctx.textBaseline = "middle";
        this.ctx.font = "30px Architects Daughter";
        this.ctx.fillStyle = "#000";

        this.ctx.fillText(
          "Area 2",
          this.width / 2 + this.width / 2 / 2,
          this.height / 2 + this.height / 2 / 2
        );
      }
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
    selectAreaOne() {
      this.clearAreaTwo();
      this.drawAreaOne("#f1f1f1", true);

      this.ctx.setLineDash([6]);
      this.ctx.strokeRect(
        this.width / 2 + 4,
        4,
        this.width / 2 - 8,
        this.height / 2 - 8
      );
    },
    selectAreaTwo() {
      this.clearAreaOne();
      this.drawAreaTwo("#f1f1f1", true);

      this.ctx.setLineDash([6]);
      this.ctx.strokeRect(
        this.width / 2 + 4,
        this.width / 2 + 4,
        this.width / 2 - 8,
        this.height / 2 - 8
      );
    },
    onMouseMove(event) {
      const rect = this.canvas.getBoundingClientRect();
      const scaleX = this.canvas.width / rect.width;
      const scaleY = this.canvas.height / rect.height;
      const x = (event.clientX - rect.left) * scaleX;
      const y = (event.clientY - rect.top) * scaleY;
      const color = "#f1f1f1";
      const fillText = true;

      if (x >= this.width / 2 && y <= this.height / 2) {
        this.drawAreaOne(color, fillText);
        this.currentArea = 1;
      } else if (x >= this.width / 2 && y >= this.height / 2) {
        this.drawAreaTwo(color, fillText);
        this.currentArea = 2;
      } else if (x <= this.width / 2) {
        this.clearArea();
        this.currentArea = 0;
      }

      if (y >= this.height / 2) {
        this.clearAreaOne();
      } else if (y <= this.height / 2) {
        this.clearAreaTwo();
      }

      if (this.selectedArea !== 0) {
        this.selectedArea === 1 ? this.selectAreaOne() : this.selectAreaTwo();
      }
    },
    selectArea() {
      this.selectedArea = this.currentArea;

      this.selectedArea === 1 ? this.selectAreaOne() : this.selectAreaTwo();

      eventBus.$emit("selectArea", this.selectedArea);
    },
    clearArea() {
      this.clearAreaOne();
      this.clearAreaTwo();

      if (this.selectedArea !== 0) {
        this.selectedArea === 1 ? this.selectAreaOne() : this.selectAreaTwo();
      }
    }
  },
  mounted() {
    // init canvas
    this.canvas = this.$refs.canvas;
    this.ctx = this.canvas.getContext("2d");
    // set background image
    this.drawBackgroundImage().then(() => {
      this.drawArea();

      eventBus.$emit("selectArea", this.selectedArea);
    });
  }
};
</script>
