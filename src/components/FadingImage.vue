<template>
  <img
    :src="imgSrc"
    alt="fading image"
    :style="{ opacity: life, width: life * 25 + '%', top: randy + 'px', left: randx + 'px', borderColor: border}"
    v-on:click="feed()"
  >
</template>

<script>
export default {
  name: "FadingImage",
  data: function() {
    return {
      life: 1.0,
      randx: 1,
      randy: 1,
      border: 'blue'
    };
  },
  props: {
    imgSrc: {
      type: String
    },
    animationID: {
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
    }
  },
  created: function () {
    // `this` points to the vm instance
    let padding = 200;
    console.log('a is: ' + this.life);
    this.randx = this.randomIntFromInterval(padding,screen.width-padding);
    this.randy = this.randomIntFromInterval(padding,screen.height-padding);
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
