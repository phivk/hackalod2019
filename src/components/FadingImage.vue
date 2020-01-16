<template>
  <img
    :src="imgURL"
    alt="fading image"
    :style="{ 
      'opacity': life, 
      'width': life * 25 + '%', 
      'top': ypos + 'px', 
      'left': xpos + 'px', 
      'z-index': zIndex
    }"
    v-on:click="feed()"
  >
</template>

<script>
export default {
  name: "FadingImage",
  data: function() {
    return {
      death_rate: 0.0015,
      life: 1.0,
      border: 'blue',
      animationID: "",
      zIndex: 1,
    };
  },
  props: {
    xpos: Number,
    ypos: Number,
    imgURL: String,
    imgURI: String,
    index: Number,
  },
  methods: {
    fade() {
      if (this.life > 0) {
        this.life -= this.death_rate;
        requestAnimationFrame(this.fade);
      } else {
        cancelAnimationFrame(this.animationID);
        this.$emit("remove", this.index, this.imgURL, this.imgURI);
      }
    },
    feed() {
      this.zIndex++
      if (this.life > 1) {
        this.breed();
      } else if (this.life > 0) {
        this.life += 0.1;
      }
    },
    breed() {
      //make children!
      this.$emit("breed", this.xpos, this.ypos, this.imgURL, this.imgURI);
    },
    point_within_circle(cx, cy, R) {
      let angle = Math.random()*Math.PI*2;
      return [(Math.cos(angle)*R) + cx,(Math.sin(angle)*R) + cy];
    }
  },
  mounted: function() {
    this.$nextTick(function() {
      this.animationID = requestAnimationFrame(this.fade);
    });
    // console.log(this.imgURI);
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
img{
  position:absolute;
  transform: translate(-50%,-50%);
  border-style: solid;
  border-width: 5px;
  z-index: 1;
}

/*.origin-rijks {
  border-color: #4089ff;
}

.origin-volks {
  border-color: #ffc940;
}*/



</style>
