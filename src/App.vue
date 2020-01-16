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
      death_rate: 0.001,
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
      const URL = "http://172.16.45.236:5000/vispa";
      // let newImage = this.get_data(URL, {uri: parent_uri, url: parent_url})[0];
      let sampleImages = this.get_data_temp()
      let randomImage = sampleImages[Math.floor(Math.random()*sampleImages.length)];
      let newImage = randomImage
      // TODO set origin- class based on origin of returned URI
      this.images.push(newImage);
    },
    // eslint-disable-next-line
    onRemove(index, imgURL, imgURI) {
      // console.log("onRemove");
      // console.log(index);
      this.$delete(this.images, index);
    },
    onSpawn(parent_url, parent_uri){
      // let URL = this.LODURLs[this.randomIntFromInterval(0,2)];
      let URL = this.LODURLs[0]; // stick to year API for now
      let newImage = this.get_data(URL, {uri: parent_uri, url: parent_url})[this.randomIntFromInterval(0,5)];
      // console.log(newImage);
      this.images.push(newImage);
    },
    randomIntFromInterval(min, max) {
      // min and max included
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
    get_data(url, data_to_send) {
      ///////////////////////////////////////////////////////
      // temporarily hard code response til API is working //
      //////////////////////////// //////////////////////////

      var tmp = null;
      $.ajax({method: 'GET',
              async: false,
              url: url, 
              data: data_to_send,
              xhrFields: {
                withCredentials: false
              },
              // eslint-disable-next-line
              success: function(data, status) {
               tmp = data;
            }})
      if(tmp !== undefined && tmp.length !== null) {
        return tmp
      }
    },
    get_data_temp() {
      ///////////////////////////////////////////////////////
      // temporarily hard code response til API is working //
      //////////////////////////// //////////////////////////
      const sampleImages = [
        {
          "@context": "https://linked.art/ns/v1/linked-art.json",
          "id": "http://hdl.handle.net/10934/RM0001.COLLECT.326",
          "representation": [
            {
              "_label": "Rijksmuseum Image API",
              "id": "https://lh3.googleusercontent.com/_DBa0zX8Vx1t8IA2oOyOnWito00trZIY7XabnX8QyKG3VSsTvNuc8hq9jyoNgPz--O9z-L71QQ-vWnQOwoDtUYjBRWkc=s0",
              "type": "VisualItem"
            }
          ],
          "type": "HumanMadeObject"
        },
        {
          "@context": "https://linked.art/ns/v1/linked-art.json",
          "id": "http://hdl.handle.net/10934/RM0001.COLLECT.327",
          "representation": [
            {
              "_label": "Rijksmuseum Image API",
              "id": "https://lh3.googleusercontent.com/3lZ51I00AIa5KZ7g0ta82UAOJsHV_Tab_OnVqTcLx_Wi2Un22F-tZzWkwcYWOqoaLEInUfce9GVIWnKfbcBkdAgcKZE=s0",
              "type": "VisualItem"
            }
          ],
          "type": "HumanMadeObject"
        }
      ]
      return sampleImages
    },
  },
  created: function() {
    // const URL = "http://172.16.45.236:5000/seed";
    // this.images = this.get_data(URL, {hello: "world!"})
    this.images = this.get_data_temp()
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

body{
  background-color: #222;
  margin: 0;
  overflow: hidden;
} 

</style>
