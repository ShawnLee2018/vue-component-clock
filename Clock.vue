<template>
   <canvas id="canvas" ref="canvas">
     canvas is not supported,please upgrade your browser.
     </canvas>    
</template>

<script>
export default {
  props: {
    size: { default: 250 },
    dialColor: { default: "#000000" },
    dialBackgroundColor: { default: "transparent" },
    dialBackgroundImage: { default: "" },
    secondHandColor: { default: "#F3A829" },
    minuteHandColor: { default: "#222222" },
    hourHandColor: { default: "#222222" },
    hourCorrection: { default: "+0" },
    showNumerals: { default: true },
    secondSetp: { default: 100 },
    smothSecondHand: { default: false }
  },
  date() {
    return {
      context: null,
      radius: 0,
      ticktock: true,
      bgImage: null,
      now() {
        return new Date();
      }
    };
  },
  methods: {
    toRadians(deg) {
      return Math.PI / 180 * deg;
    },
    drawDial(color, bgcolor) {
      let i;
      let ang;
      let sang;
      let cang;
      let sx;
      let sy;
      let ex;
      let ey;
      let nx;
      let ny;
      let text;
      let textSize;
      let textWidth;
      const dialRadius = parseInt(this.radius - this.size / 50, 10);
      const dialBackRadius = this.radius - this.size / 400;

      this.context.beginPath();
      this.context.arc(0, 0, dialBackRadius, 0, 360, false);
      this.context.fillStyle = bgcolor;
      this.context.fill();
      if (this.bgImage != null && this.bgImage.complete) {
        this.context.save();
         this.context.globalAlpha =0.5;
        this.context.beginPath();
        this.context.arc(0, 0, dialBackRadius * 0.5, 0, 360);
        this.context.clip();
        this.context.drawImage(
          this.bgImage,
          -this.size * 0.25,
          -this.size * 0.25,
          this.size * 0.5,
          this.size * 0.5
        );
        this.context.restore();
      }
      for (i = 1; i <= 60; i += 1) {
        ang = Math.PI / 30 * i;
        sang = Math.sin(ang);
        cang = Math.cos(ang);
        if (i % 5 === 0) {
          this.context.lineWidth = parseInt(this.size / 50, 10);
          sx = sang * (dialRadius - dialRadius / 9);
          sy = cang * -(dialRadius - dialRadius / 9);
          ex = sang * dialRadius;
          ey = cang * -dialRadius;
          nx = sang * (dialRadius - dialRadius / 4.2);
          ny = cang * -(dialRadius - dialRadius / 4.2);
          text = i / 5;
          this.context.textBaseline = "middle";
          textSize = parseInt(this.size / 13, 10);
          this.context.font = `100 ${textSize}px helvetica`;
          textWidth = this.context.measureText(text).width;
          this.context.beginPath();
          this.context.fillStyle = color;
          if (this.showNumerals) {
            this.context.fillText(text, nx - textWidth / 2, ny);
          }
        } else {
          this.context.lineWidth = parseInt(this.size / 100, 10);
          sx = sang * (dialRadius - dialRadius / 20);
          sy = cang * -(dialRadius - dialRadius / 20);
          ex = sang * dialRadius;
          ey = cang * -dialRadius;
        }
        this.context.beginPath();
        this.context.strokeStyle = color;
        this.context.lineCap = "round";
        this.context.moveTo(sx, sy);
        this.context.lineTo(ex, ey);
        this.context.stroke();
      }
    },
    twelvebased(hour) {
      let h = hour;
      if (hour >= 12) {
        h -= 12;
      }
      return h;
    },
    drawHand(length) {
      this.context.beginPath();
      this.context.moveTo(0, 0);
      this.context.lineTo(0, length * -1);
      this.context.stroke();
    },
    drawSecondHand(seconds, color) {
      const shlength = this.radius - this.size / 40;
      this.context.save();
      this.context.lineWidth = parseInt(this.size / 150, 10);
      this.context.lineCap = "round";
      this.context.strokeStyle = color;
      this.context.rotate(this.toRadians(seconds * 6));
      this.context.shadowColor = "rgba(0,0,0,.5)";
      this.context.shadowBlur = parseInt(this.size / 80, 10);
      this.context.shadowOffsetX = parseInt(this.size / 200, 10);
      this.context.shadowOffsetY = parseInt(this.size / 200, 10);
      this.drawHand(shlength);
      this.context.beginPath();
      this.context.moveTo(0, 0);
      this.context.lineTo(0, shlength / 15);
      this.context.lineWidth = parseInt(this.size / 30, 10);
      this.context.stroke();
      this.context.beginPath();
      this.context.arc(0, 0, parseInt(this.size / 30, 10), 0, 360, false);
      this.context.fillStyle = color;
      this.context.fill();
      this.context.restore();
    },
    drawMinuteHand(minutes, color) {
      const mhlength = this.size / 2.2;
      this.context.save();
      this.context.lineWidth = parseInt(this.size / 50, 10);
      this.context.lineCap = "round";
      this.context.strokeStyle = color;
      this.context.rotate(this.toRadians(minutes * 6));
      this.context.shadowColor = "rgba(0,0,0,.5)";
      this.context.shadowBlur = parseInt(this.size / 50, 10);
      this.context.shadowOffsetX = parseInt(this.size / 250, 10);
      this.context.shadowOffsetY = parseInt(this.size / 250, 10);
      this.drawHand(mhlength);
      this.context.restore();
    },
    drawHourHand(hours, color) {
      const hhlength = this.size / 3;
      this.context.save();
      this.context.lineWidth = parseInt(this.size / 25, 10);
      this.context.lineCap = "round";
      this.context.strokeStyle = color;
      this.context.rotate(this.toRadians(hours * 30));
      this.context.shadowColor = "rgba(0,0,0,.5)";
      this.context.shadowBlur = parseInt(this.size / 50, 10);
      this.context.shadowOffsetX = parseInt(this.size / 300, 10);
      this.context.shadowOffsetY = parseInt(this.size / 300, 10);
      this.drawHand(hhlength);
      this.context.restore();
    },
    timeToDecimal(time) {
      let h = 0;
      let m = 0;
      if (time !== undefined) {
        h = this.twelvebased(time.getHours());
        m = time.getMinutes();
      }
      return parseInt(h, 10) + m / 60;
    },
    numberCorrection(num) {
      if (num !== "+0" && num !== "") {
        if (num.charAt(0) === "+") {
          return +num.charAt(1);
        }
        return -num.charAt(1);
      }
      return 0;
    },
    startClock() {
      const theDate = new Date();
      const s = theDate.getSeconds();
      const ms = theDate.getMilliseconds();
      const mins = theDate.getMinutes();
      const m = mins + s / 60;
      const hours = theDate.getHours();
      const h =
        this.twelvebased(hours + this.numberCorrection(this.hourCorrection)) +
        m / 60;
      this.context.clearRect(-this.radius, -this.radius, this.size, this.size);
      this.drawDial(this.dialColor, this.dialBackgroundColor);

      this.drawHourHand(h, this.hourHandColor);
      this.drawMinuteHand(m, this.minuteHandColor);
      if (this.smothSecondHand) {
        this.drawSecondHand(s + ms / 1000, this.secondHandColor);
      } else if (ms < 800) {
        this.drawSecondHand(s, this.secondHandColor);
      } else if (ms >= 800 && ms < 900) {
        this.drawSecondHand(s + 1.05, this.secondHandColor);
      } else if (ms >= 900) {
        this.drawSecondHand(s + 0.98, this.secondHandColor);
      }

      if (window.requestAnimationFrame) {
        window.requestAnimationFrame(this.startClock);
      } else {
        setTimeout(() => {
          this.startClock();
        }, this.secondSetp);
      }
    }
  },
  created() {
    this.bgImage = new Image();
    this.bgImage.src = this.dialBackgroundImage;
  },
  mounted() {
    const cnv = this.$refs.canvas;
    this.context = cnv.getContext("2d");
    cnv.width = this.size;
    cnv.height = this.size;
    this.radius = parseInt(this.size / 2, 10);
    this.context.translate(this.radius, this.radius);

    this.startClock();
  }
};
</script> 
<style scoped>
</style>
