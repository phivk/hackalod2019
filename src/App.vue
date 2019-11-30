<template>
  <div id="app">
    <FadingImage
      v-for="(img, index) in images"
      :imgSrc="img.representation[0].id"
      :key="img.id+Math.random().toString()"
      v-on:breed="onBreed"
      v-on:remove="onRemove(index)"
    />
  </div>
</template>

<script>
import FadingImage from "./components/FadingImage";
import JQuery from "jquery";
let $ = JQuery;

export default {
  name: "App",
  components: {
    FadingImage
  },
  data: function() {
    return {
      images: [],
      LODURLs: [
        "https://v2.bencomp.nl/year",
        "https://v2.bencomp.nl/location",
        "https://v2.bencomp.nl/technique",
      ]
    };
  },
  methods: {
    onBreed() {
      console.log("birth!");
    },
    onRemove(index) {
      console.log("REMOVE");
      this.images.splice(index, 1);
      this.spawn();
    },
    spawn(){
      let URL = this.LODURLs[this.randomIntFromInterval(0,2)];
      let newImage = this.get_data(URL, {uri: "https://hdl.handle.net/20.500.11840/1"})[0];
      console.log(newImage);
      this.images.push(newImage);
    },
    randomIntFromInterval(min, max) {
      // min and max included
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
    get_data(url, data_to_send) {
      var tmp = null;
      $.ajax({method: 'GET',
              async: false,
              url: url, 
              data: data_to_send,
              xhrFields: {
                withCredentials: false
              },
              success: function(data, status) {
               tmp = data;
            }})
      if(tmp !== undefined && tmp.length !== null) {
        return tmp
      }
    },
  },
  created: function() {
    const URL = "http://172.16.45.236:5000/seed";
    this.images = this.get_data(URL, {hello: "world!"})
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
body{background-color: #222;}

</style>
