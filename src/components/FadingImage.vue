<template>
  <img
    :src="imgSrc"
    alt="fading image"
    :style="{ opacity: life, width: life * 25 + '%', top: ypos + 'px', left: xpos + 'px', borderColor: border}"
    v-on:click="feed()"
  >
</template>

<script>
export default {
  name: "FadingImage",
  data: function() {
    return {
      life: 1.0,
      xpos: 1,
      ypos: 1,
      border: 'blue',
      animationID: ""
    };
  },
  props: {
    imgSrc: {
      type: String
    }
  },
  methods: {
    fade() {
      if (this.life > 0) {
        this.life -= 0.001;
        requestAnimationFrame(this.fade);
      } else {
        cancelAnimationFrame(this.animationID);
        this.$emit("remove");
      }
    },
    feed() {
      if (this.life > 1) {
        this.breed();
      } else if (this.life > 0) {
        this.life += 0.1;
      }
    },
    breed() {
      //make children!
      this.$emit("breed");
      this.tint = 1;
    },
    randomIntFromInterval(min, max) {
      // min and max included
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
    point_within_circle(cx, cy, R) {
	   var angle = Math.random()*Math.PI*2
	   return [(Math.cos(angle)*R) + cx,(Math.sin(angle)*R) + cy]
       }
  },
  created: function () {
    // `this` points to the vm instance
    let padding = 200;
    console.log('a is: ' + this.life);
    this.xpos = this.randomIntFromInterval(padding,screen.width-padding);
    this.ypos = this.randomIntFromInterval(padding,screen.height-padding);
  },
  mounted: function() {
    this.$nextTick(function() {
      this.animationID = requestAnimationFrame(this.fade);
    });
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
img{position:absolute;
transform: translate(-50%,-50%);
border-style: solid;
border-width: 10px;
}
</style>
