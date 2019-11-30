<template>
  <div id="app">

    <div id="gui_container">
      <div id="gui">
          <h1> Niet te Vergeten
           </h1>

      </div>
    </div>

    <div id="images_container">

    <FadingImage
      v-for="(img, index) in images"
      :imgURL="img.representation[0].id"
      :imgURI="img.id"
      :key="img.id+Math.random().toString()"
      :index="index"
      v-on:breed="onBreed"
      v-on:remove="onRemove"
    />
  </div>
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
    onBreed(xpos, ypos, parent_url, parent_uri) {
      console.log("birth! " + xpos + " " + ypos);
      const URL = "http://172.16.45.236:5000/vispa";
      var newImage = this.get_data(URL, {uri: parent_uri, url: parent_url})[0];
      console.log(newImage);
      this.images.push(newImage);
    },
    onRemove(index, imgURL, imgURI) {
      console.log("onRemove");
      console.log(index);
      this.$delete(this.images, index);
    },
    onSpawn(parent_url, parent_uri){
      // let URL = this.LODURLs[this.randomIntFromInterval(0,2)];
      let URL = this.LODURLs[0];
      let newImage = this.get_data(URL, {uri: parent_uri, url: parent_url})[this.randomIntFromInterval(0,5)];
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
  position: relative;
  margin-top: 0;
}

#gui_container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 10000;
  display: flex;
  justify-content: flex-end;
}

h1 {
  font-family: "Avenir", Helvetica, sans-serif;
  text-size: 22px;
  text-align: center;
  color: black;
  margin: 30px 0 30px 0;
  line-height: 36px;
}

p {
  margin: 0;
  text-align: left;
  text-size: 22px;
}

#gui {
  padding: 0 35px;
  background-color: white;
}

#images_container {
  position: relative;
  z-index: 1;
}

body{background-color: #222;}

</style>
